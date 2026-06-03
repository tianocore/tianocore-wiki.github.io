# Beagleboardpkg

The [Beagle Board](https://beagleboard.org/) is a low cost highly capable
single board computer. The Beagle Board is based on an OMAP3530 SoC
featuring an ARM® CortexTM-A8 processor. Please go
[here](https://www.beagleboard.org/buy), to get information on how to buy a
Beagle Board. Don't forget to also order a serial cable and power
supply.
The BeagleBoardPkg package reuses components from ArmPkg and
Omap35xxPkg.

## Source Repository

[1](https://github.com/tianocore/edk2/tree/master/BeagleBoardPkg)

## Developer References

- [Beagle Board System Reference
  Manual](https://beagleboard.org/static/BBSRM_latest.pdf) (Chapter 12
  talks about booting)
- [OMAP 35x Peripherals Overview Reference
  Guide](https://focus.ti.com/general/docs/lit/getliterature.tsp?literatureNumber=sprufn0a&fileType=pdf)
  (Chapter 25 talks about booting)

## BeagleBoard Support and Status

- Tested revision 12923 (Tue Jan 10 2012)
- NAND, SD Card, USB EHCI controller, Display controller EFI Drivers are
  fully functional.
- Boot Linux zImage with ATAG or Device Tree blob (FDT)

## How to build UEFI for the BeagleBoard

1\. Get the requirements

- A Universally Unique Id (UUID) header. Needed to build the EDK2
  BaseTools. On Ubuntu: `sudo apt-get install uuid-dev`

2\. Download the sources

    svn co https://svn.code.sf.net/p/edk2/code/trunk/edk2 edk2 --username guest
    cd $(WORKROOT)/edk2
    svn co https://svn.code.sf.net/p/edk2-fatdriver2/code FatPkg --username guest
    patch -p1 < ArmPlatformPkg/Documentation/patches/BaseTools-Pending-Patches.patch
    cd BeagleBoardPkg/

3\. Get the toolchain: See [ArmPkg-Toolchain](../../build-tooling/environment-setup/armpkg_toolchain.md)

4\. Apply the ARM pending patches required to build the firmware

    cd $(WORKROOT)/edk2
    patch -p0 < ArmPlatformPkg/Documentation/patches/BaseTools-Pending-Patches.patch

5\. Build the Release build of UEFI firmware:

    cd BeagleBoardPkg/
    ./build.sh RELEASE

- To Build the Debug version of UEFI

    ./build.sh

## Using UEFI to boot Linux

1\. Installing Linaro image Creator:

- Getting from Linaro website:

wget
[https://launchpad.net/linaro-image-tools/trunk/0.4.8/+download/linaro-image-tools-0.4.8.tar.gz](https://launchpad.net/linaro-image-tools/trunk/0.4.8/+download/linaro-image-tools-0.4.8.tar.gz)
    tar xzf linaro-image-tools-0.4.8.tar.gz
    cd $(WORKROOT)/linaro-image-tools-0.4.8/
sudo apt-get install parted dosfstools uboot-mkimage python-argparse python-dbus python-debian python-parted
qemu-arm-static btrfs-tools command-not-found

Add linaro-media-create path to the PATH environment variable

- Getting from the Ubuntu package:

    sudo add-apt-repository ppa:linaro-maintainers/tools
    sudo apt-get update
    sudo apt-get install linaro-image-tools

2\. Creating the u-boot + Linux Linaro image:

    mkdir $(WORKROOT)/beagle_image && cd $(WORKROOT)/beagle_image
    wget https://releases.linaro.org/platform/linaro-m/hwpacks/final/hwpack_linaro-omap3_20101109-1_armel_supported.tar.gz
    wget https://releases.linaro.org/platform/linaro-m/headless/release-candidate/linaro-m-headless-tar-20101101-0.tar.gz
sudo linaro-media-create --image_file beagle_sd.img --dev beagle --binary linaro-m-headless-tar-20101101-0.tar.gz
--hwpack hwpack_linaro-omap3_20101109-1_armel_supported.tar.gz
    sudo chmod a+rw beagle_sd.img

3\. Replacing u-boot by UEFI

    ```text
    mkdir /tmp/beagle_boot
    sudo mount -o loop,offset=$[63*512] $(WORKROOT)/beagle_image/beagle_sd.img /tmp/beagle_boot
    sudo cp $(WORKROOT)/edk2/Build/BeagleBoard/RELEASE_ARMGCC/FV/BEAGLEBOARD_EFI.fd /tmp/beagle_boot/u-boot.bin
    sudo umount /tmp/beagle_boot
    ```

4\. ARM UEFI currently only support zImage. Add the zImage to the
sdcard:

    ```text
    tar xzf hwpack_linaro-omap3_20101109-1_armel_supported.tar.gz
    cd pkgs/
    dpkg -x linux-image-2.6.35-1008-linaro-omap_2.6.35-1008.15_armel.deb .
    sudo mount -o loop,offset=$[63*512] $(WORKROOT)/beagle_image/beagle_sd.img /tmp/beagle_boot
    sudo cp boot/vmlinuz-2.6.35-1008-linaro-omap /tmp/beagle_boot/zImage
    ```

5\. Replace the Linux command line

- Get the command line from uboot ('boot.txt' from the boot partition):

    console=ttyS2,115200n8 root=/dev/mmcblk0p2 rw earlyprintk fixrtc nocompcache vram=12M omapfb.mode=dvi:1280x720MR-16@60

- Start the UEFI on the BeagleBoard
- Go into the 'Boot Menu'
- Update the existing entry

      ```text
      The default boot selection will start in   8 seconds
      [1] Linux from SD
      [2] EBL
      [3] Boot Manager
      Start: 3
      [1] Add Boot Device Entry
      [2] Update Boot Device Entry
      [3] Remove Boot Device Entry
      [4] Return to main menu
      Choice: 2
      [1] Linux from SD
      Update entry: 1
      File path of the EFI Application or the kernel: zImage
      Arguments to pass to the binary: console=ttyS2,115200n8 root=/dev/mmcblk0p2 rw earlyprintk fixrtc nocompcache vram=12M
      omapfb.mode=dvi:1280x720MR-16@60
      Description for this new Entry: Linux from SD
      [1] Add Boot Device Entry
      [2] Update Boot Device Entry
      [3] Remove Boot Device Entry
      [4] Return to main menu
      Choice: 4
      [1] Linux from SD
      [2] EBL
      [3] Boot Manager
      Start: 1
        DXE    498 ms
        BDS    291 ms
      Total Time = 790 ms
      Uncompressing Linux... done, booting the kernel.
      [    0.000000] Initializing cgroup subsys cpuset
      [    0.000000] Initializing cgroup subsys cpu
      [ 0.000000] Linux version 2.6.35-1008-linaro-omap (buildd@hawthorn) (gcc version 4.4.5 (Ubuntu/Linaro 4.4.4-14ubuntu5) )
      #15-Ubuntu Fri Oct 22 11:56:29 UTC 2010 (Ubuntu 2.6.35-1008.15-linaro-omap 2.6.35.7)
      [    0.000000] CPU: ARMv7 Processor [412fc083] revision 3 (ARMv7), cr=10c53c7f
      [    0.000000] CPU: VIPT nonaliasing data cache, VIPT nonaliasing instruction cache
      [    0.000000] Machine: OMAP3 Beagle Board
      [    0.000000] bootconsole [earlycon0] enabled
      ```

The EBL (EDK Boot Loader) is a small simple command line environment
that is much simpler that the EFI shell. It is also possible to launch
the EFI shell from the EBL command prompt. The EBL source code is
located in the edk2 EmbeddedPkg.
Please see the [EDK Boot Loader (EBL) WiKi](../../archives/reference/edk_boot_loader_ebl_wiki.md) for
more information.

## Testing UEFI on qEmu

    sudo add-apt-repository ppa:linaro-maintainers/tools
    sudo apt-get update
    sudo apt-get install qemu-user-static qemu-system

- Using the previous image:

    qemu-system-arm -M beagle -serial stdio -sd beagle_sd.img

- Booting UEFI from NOR Flash: Rebuild the UEFI firmware WITH the
  OMAP353x header:

    cd $(WORKROOT)/edk2/BeagleBoardPkg/
    ./build.sh RELEASE
qemu-system-arm -M beagle -serial stdio -sd beagle_sd.img -mtdblock
$(WORKROOT)/edk2/Build/BeagleBoard/RELEASE_ARMGCC/FV/BeagleBoard_EFI_flashboot.fd

- If UEFI fails to find the Linux kernel then you should expect to see
  this error message:

    ERROR : Did not find linux kernel.

Two reasons could be the source of the error. Either there is no zImage
on the SD card (the default location of the kernel for this version of
UEFI) or the Device Path of the SD card has changed.

To check if the zImage is on the SD card, start EBL and browse the SD
card file system to ensure a 'zImage' file exists on the root of your
boot partition:

    BeagleEdk2 >device
    Firmware Volume Devices:
      fv0: 0x80008000 - 0x80087FFF : 0x00080000
      fv1: 0x87BAF000 - 0x87DC1F37 : 0x00212F38
    File System Devices:
      fs0: boot:
    Block IO Devices:
      blk0: Size = 0x10000000
      blk1: Removable Size = 0xC0000000
      blk2: fs0: Removable Partition Size = 0x33F8000
      blk3: Removable Partition Size = 0xBCC00000
    BeagleEdk2 >cd fs0:
    BeagleEdk2 fs0:\>dir
           524,288 u-boot.bin
            18,360 MLO
         3,704,948 uImage
         4,765,522 uInitrd
               387 boot.txt
               459 boot.scr
               459 boot.ini
         3,704,884 zImage
    BeagleEdk2 fs0:\>

And to list the Device Paths supported by your firmware you can also use
the command 'devicepath' of EBL:

    ```text
    BeagleEdk2 fs0:\>devicepath
    [0x87964690] MemoryMapped(0xB,0x80008000,0x80087FFF)
    [0x87964390] MemoryMapped(0xB,0x87BAF000,0x87DC1F37)
    [0x87064B10] VenHw(D3987D4B-971A-435F-8CAF-4967EB627241)/Uart(115200,8,N,1)
    [0x87062F10] VenHw(4D00EF14-C4E0-426B-81B7-30A00A14AAD6)
    [0x8705D410] VenHw(100C2CFA-B586-4198-9B4C-1683D195B1DA)
    [0x8705C890] PciRoot(0x0)/Pci(0x0,0x0)
    [0x8705BD10] VenHw(E68088EF-D1A4-4336-C1DB-4D3A204730A6)
    [0x87052A90] VenHw(D3987D4B-971A-435F-8CAF-4967EB627241)/Uart(115200,8,N,1)/VenPcAnsi()
    [0x8704A710] VenHw(100C2CFA-B586-4198-9B4C-1683D195B1DA)/HD(1,MBR,0x00000000,0x3F,0x19FC0)
    [0x8704A410] VenHw(100C2CFA-B586-4198-9B4C-1683D195B1DA)/HD(2,MBR,0x00000000,0x1A000,0x5E6000)
    BeagleEdk2 fs0:\>
    ```

The default kernel is expected at
VenHw(B615F1F5-5088-43CD-809C-A16E52487D00)/HD(1,MBR,0x00000000,0x3F,0x19FC0)/zImage
This is defined by BeagleBoardPkg/BeagleBoardPkg.dsc:

    gArmPlatformTokenSpaceGuid.PcdDefaultBootDevicePath|L"VenHw(B615F1F5-5088-43CD-809C-A16E52487D00)/HD(1,MBR,0x00000000,0x3F,0x19FC0)/zImage"

## Booting EDK2 in the Beagle Board DRAM using U-Boot

1\. Build the BeagleBoard EDK2 as defined in section
[How to build UEFI for the BeagleBoard](#how-to-build-uefi-for-the-beagleboard).

2\. Copy the BeagleBoard UEFI firmware (BEAGLEBOARD_EFI.fd) on your boot
partition (the FAT partition) of your SD card.

3\. Start U-Boot on the BeagleBoard

4\. Load and Start the UEFI Firmware

    ```text
    OMAP3 beagleboard.org # mmc init
    mmc1 is available
    OMAP3 beagleboard.org # fatls mmc1
    usage: fatls <interface> <dev[:part]> [directory]
    OMAP3 beagleboard.org # fatls mmc 1
    Unknown command 'fatls' - try 'help'
    OMAP3 beagleboard.org # fatls mmc 1 /
       192204   u-boot.bin
        18360   mlo
      3704948   uimage
      4765782   uinitrd
          387   boot.txt
          459   boot.scr
          459   boot.ini
       524288   BEAGLEBOARD_EFI.fd

    8 file(s), 0 dir(s)

    OMAP3 beagleboard.org # fatload mmc 1 80008000 BEAGLEBOARD_EFI.fd
    reading BEAGLEBOARD_EFI.fd

    524288 bytes read
    OMAP3 beagleboard.org # go 80008000
    ## Starting application at 0x80008000 ...
    Magic delay to disable watchdog timers properly.
    UEFI firmware built at 12:34:19 on Nov 16 2011
    add-symbol-file
    /home/olivier/tianocore-svn/Build/BeagleBoard/DEBUG_ARMLINUXGCC/ARM/MdeModulePkg/Core/Dxe/DxeMain/DEBUG/DxeCore.dll
    0x87B88240
        Loading DxeCore at 0x0087B88000 EntryPoint=0x0087B88241
    add-symbol-file
    /home/olivier/tianocore-svn/Build/BeagleBoard/DEBUG_ARMLINUXGCC/ARM/MdeModulePkg/Core/Dxe/DxeMain/DEBUG/DxeCore.dll
    0x87B88004
        HOBLIST address in DXE = 0x87B70010
        Memory Allocation 0x00000004 0x87FEF000 - 0x87FEFFFF
        Memory Allocation 0x00000004 0x87FE7000 - 0x87FEEFFF
        Memory Allocation 0x00000004 0x87FF0000 - 0x87FFFFFF
        Memory Allocation 0x00000004 0x87FD7000 - 0x87FE6FFF
        (...)
        The default boot selection will start in  10 seconds
        [1] Linux from SD
            - VenHw(B615F1F5-5088-43CD-809C-A16E52487D00)/HD(1,MBR,0x00000000,0x3F,0x19FC0)/zImage
            - Arguments: console=tty0 console=ttyS2,115200n8 root=UUID=a4af765b-c2b5-48f4-9564-7a4e9104c4f6 rootwait ro
              earlyprintk
            - LoaderType: 1
        [2] EBL
        [3] Boot Manager
        Start:
    ```

1. It should also be possible to boot EFI from USB using the following
    U-Boot commands:

    OMAP3 beagleboard.org # usb reset
    OMAP3 beagleboard.org # usb scan
    OMAP3 beagleboard.org # fatload USB 0:1 80008000 BEAGLEBOARD_EFI.fd
    OMAP3 beagleboard.org # go 80008000
