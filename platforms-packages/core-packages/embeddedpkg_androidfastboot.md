# EmbeddedPkg Android FastBoot

Fastboot is a protocol designed as a mechanism to update Android
platforms. This page describes an implementation of the device-side as
part of UEFI. You can use it to download and flash images onto a
target's storage, or to download and boot Linux kernels.

Android FastBoot support in UEFI has been added into SVN rev15317
(2014-03-05). The protocol is documented in the Android source tree in
\`system/core/fastboot/fastboot_protocol.txt\`:
[https://android.googlesource.com/platform/system/core/+/master/fastboot/fastboot_protocol.txt](https://android.googlesource.com/platform/system/core/+/master/fastboot/fastboot_protocol.txt)

The Android FastBoot host tool is available from the Android ADT
(Android Developer Tools) at
[https://developer.android.com/sdk/index.html](https://developer.android.com/sdk/index.html)

## Build and Start Android FastBoot on UEFI

### Building instructions

1\. Build UEFI for Versatile Express TC2. See build instructions in this
[wiki
page](https://sourceforge.net/apps/mediawiki/tianocore/index.php?title=ArmPlatformPkg/ArmVExpressPkg#Starting_UEFI_from_ARM_Boot_Monitor_on_VExpress_Core_Tile_A15x2-A7x3)

2\. Copy UEFI FastBoot Application into NOR Flash

    cp Build/ArmVExpress-CTA15-A7/DEBUG_GCC48/ARM/AndroidFastbootApp.efi /media/ELBA/SOFTWARE/FastBoot.efi

3\. Add FastBoot.efi to your /SITE1/HBI0249A/images.txt. Example:

    [IMAGES]
    TOTALIMAGES: 7                   ;Number of Images (Max: 32)

    (...)

    NOR6UPDATE: AUTO                 ;IMAGE UPDATE:NONE/AUTO/FORCE
    NOR6ADDRESS: 00000000            ;Image Flash Address
    NOR6NAME: fastboot.efi           ;Image Flash Name
    NOR6FILE: \SOFTWARE\fastboot.efi ;Image File Name
    NOR6LOAD: 00000000               ;Image Load Address
    NOR6ENTRY: 00000000              ;Image Entry Point

### Preparing Android media

On ARM Versatile Express, we assume the SD card would contain the
Android Partitions that would be exposed by the Android FastBoot
protocol.

Prepare an SD card with an Android build for VExpress according to
Linaro's instructions:
[https://wiki.linaro.org/Platform/Android/ImageInstallation?action=show&amp;redirect=Platform%2FAndroid%2FInstallImages](https://wiki.linaro.org/Platform/Android/ImageInstallation?action=show&amp;redirect=Platform%2FAndroid%2FInstallImages)

At time of writing, Linaro's android build uses the legacy MBR partition
table, which is not supported by Fastboot on VExpress. We will now
convert the SD card from MBR to GPT.

GPT requires a backup copy of the partition table at the end of the
media. When converting from MBR to GPT, this poses a problem as the last
partition extends to the end of the device, not leaving space for the
backup GPT. To get around this, we will need to resize the last
partition. Instructions follow to do so with GNU Parted. \$DEV is the
device name of your SD card - it will look like \`/dev/sdc\` (not
/dev/sdc1).

1\. Run \`parted \$DEV\` as root.

2\. Type \`print\` to show the partition table.

3\. Identify the start and end of the last partition.

4\. Subtract 2MB from the end of the partition, and type \`resize
\$START \$END\`, where \$START is the start of the partition and \$END
is the new end you calculated. Include the "MB" suffix in \$START and
\$END.

5\. Type \`quit\`.

6\. Now use \`gdisk\` to convert from MBR to GPT.

7\. Run \`gdisk \$DEV\` as root.
You should see a warning stating that the media is partitioned with MBR,
and that gdisk will convert to GPT. Type \`w\` to write the partition
table, converting to GPT, and quit.

In order for the VExpress Fastboot platform to identify partitions, we
also need to set the Partition Name fields in the GPT. The reason for
this is that flashing a partition will potentially change the
filesystem's name, whereas the Partition Name is constant. We will now
go through the partition table and populate these fields so that each
partition's Partition Name is the same as the name of the filesystem on
that partition.

8\. Run \`ls -l /dev/disk/by-label/\` to see the filesystem labels for
each partition. This directory contains symbolic links, named according
to the name of the filesystem on the partition, to each partition's
device file.

9\. Run \`gdisk \$DEV\` as root (again).

10\. For each partition containing a filesystem with a name, type \`c\`,
select the partition by number, then type the filesystem's name.
Type \`w\` to save the changes and quit.

(In the future, Linaro's Android builds will automatically use GPT and
set the Partition Name field).

### Use Android FastBoot

1\. Plug your ARM Versatile Express to your host machine via USB (using
the Mini-USB connection on the VExpress).
Insert the SD card into the ARM Versatile Express SD slot.

2\. power up your Versatile Express.

3\. Add and Start UEFI FastBoot Application to Boot entries:

    [1] NorFlash
    [2] Linux from BootMonFs
    [3] Shell
    [4] Boot Manager
    Start: 4
    [1] Add Boot Device Entry
    [2] Update Boot Device Entry
    [3] Remove Boot Device Entry
    [4] Update FDT path
    [5] Return to main menu
    Choice: 1
    [1] NOR Flash (63 MB)
    [2] NOR Flash (63 MB)
    [3] VenHw(E7223039-5836-41E1-B542-D7EC736C5E59)
    [4] VenHw(02118005-9DA7-443A-92D5-781F022AEDBB)
    [5] VenHw(1F15DA3C-37FF-4070-B471-BB4AF12A724A)
    [6] VenHw(CC2CBF29-1498-4CDD-8171-F8B6B41D0909)
    [7] VenHw(09831032-6FA3-4484-AF4F-0A000A8D3A82)
    Select the Boot Device: 1
    File path of the EFI Application or the kernel: fastboot.efi
    Is your application is an OS loader? [y/n] n
    Description for this new Entry: Android Fastboot
    [1] Add Boot Device Entry
    [2] Update Boot Device Entry
    [3] Remove Boot Device Entry
    [4] Update FDT path
    [5] Return to main menu
    Choice: 5
    [1] NorFlash
    [2] Linux from BootMonFs
    [3] Android Fastboot
    [4] Shell
    [5] Boot Manager
    Start: 3
EDK2 USB Fastboot mode: this device reports 0xf00d for its device ID. If using the Android SDK fastboot host
application, add '-i 0xf00d' to the command line arguments.
    Android Fastboot mode - version 0.4. Press any key to quit.

4\. Your ARM Versatile Express should now be in Fastboot mode, and you
should be able to run fastboot commands from the host. You will need to
add \`-i 0xf00d\` to the fastboot command, and you will probably need to
run it as root. Some example commands to try:

- \`sudo fastboot -i 0xf00d boot zImage\` (replace zImage with a Linux
  kernel supporting FDT) - this will download and boot the kernel.
- \`sudo fastboot -i 0xf00d flash sdcard test_fs.dat\` (replace
  test_fs.dat with a test FAT filesystem image) - this will flash the
  image onto the "sdcard" partition.

## Adding/Porting Android FastBoot to your UEFI firmware

### Overview

The implementation is distributed into several components:

- EmbeddedPkg/Application/AndroidFastbootApp is the platform-independent
  UEFI application that handles the core logic of a Fastboot session. It
  uses the FASTBOOT_TRANSPORT_PROTOCOL to communicate with the host and
  the FASTBOOT_PLATFORM_PROTOCOL to initiate platform-specific
  operations.

- EmbeddedPkg/Drivers/AndroidFastbootTransportUsbDxe is a driver
  implementing the FASTBOOT_TRANSPORT_PROTOCOL over USB by acting as a
  peripheral. This requires a USB peripheral or OTG controller and a
  UEFI driver for that controller. USB is the "default" transport system
  for Fastboot - the only one supported by the core AOSP.

- ArmPlatformPkg/ArmVExpressPkg/ArmVExpressFastBootDxe is an example of
  a FASTBOOT_PLATFORM_PROTOCOL implementation for ARM development
  platforms. It uses the SD card as the "flash memory".

![](UefiAndroidFastboot.PNG)
*UefiAndroidFastboot.PNG*

### Porting

To port UEFI Fastboot to your platform you will need implementations of
the FASTBOOT_PLATFORM_PROTOCOL and the FASTBOOT_TRANSPORT_PROTOCOL.
These UEFI protocols are documented in their respective header files
under \`EmbeddedPkg/Include/Protocol\`.

The FASTBOOT_PLATFORM_PROTOCOL's main responsibility is to handle the
mapping from Fastboot's notion of "partition names" to actual partitions
on the platform's storage media. It also handles OEM-specific Fastboot
commands.

The FASTBOOT_TRANSPORT_PROTOCOL provides an abstracted means for
AndroidFastbootApp to communicate with the host.

In order to use Fastboot over USB on your platform, you will need to
write a UEFI driver for the USB peripheral controller which exposes the
USB_DEVICE_PROTOCOL defined in
\`EmbeddedPkg/Include/Protocol/UsbDevice.h\`. At time of writing, this
protocol consists of the bare minimum of functionality required to
implement Fastboot. You can then use UsbFastbootTransportDxe, which will
consume the USB_DEVICE_PROTOCOL and produce the
FASTBOOT_TRANSPORT_PROTOCOL.
