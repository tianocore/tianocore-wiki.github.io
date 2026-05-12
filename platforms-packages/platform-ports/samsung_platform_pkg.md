# SamsungPlatformPkg

The Samsung Origen ([https://www.origenboard.org/](https://www.origenboard.org/)) is one of the low-cost
ARM development boards. The platform is powered by a dual Cortex A9 and
supports a large range of devices on boards (2x SD card, LCD & HDMI
connectors, USB 2.0, etc).

### Status

Tested on the revisions:

- EDK2: 13012 (14/02/2012)
- SamsungPlatformPkg: commit: ed456b2d64f3 (22/11/2011)

### Build EDK2 for Samsung Origen

1\. The EDK2 Samsung repository is currently located in a third-party
repository. To get the sources:

    svn co https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2 edk2 --username guest
    cd $(WORKROOT)/edk2
    svn co https://edk2-fatdriver2.svn.sourceforge.net/svnroot/edk2-fatdriver2/trunk/FatPkg FatPkg --username guest
    git clone git://github.com/girishks/Origen-EDK-II-Package.git
    mv Origen-EDK-II-Package/SamsungPlatformPkg .

2\. Apply the latest patches:

    patch -p1 < 0001-SamsungPlatformPkg-ExynosPkg-Do-not-initialize-the-T.patch
    patch -p1 < 0002-SamsungPlatformPkg-TimerDxe-Do-not-overwrite-Timers-.patch
    patch -p1 < 0003-SamsungPlatformPkg-ExynosPkg-Fixed-UART-Rx-Ready-Fla.patch
    patch -p1 < 0004-SamsungPlatformPkg-OrigenBoardPkg-Update-Linux-infor.patch
    patch -p1 < 0005-SamsungPlatformPkg-OrigenBoardPkg-Removed-deprecated.patch
    patch -p1 < 0006-SamsungPlatformPkg-SmdkBoardPkg-Removed-memory-initi.patch
    patch -p1 < 0007-SamsungPlatformPkg-SmdkBoardLib-Fixed-GCC-build-issu.patch

3\. Add support for one of the supported toolchains. See
[ArmPkg-Toolchain](../../build-tooling/environment-setup/armpkg_toolchain.md)

4\. Build EDK2:

    . edksetup.sh
    make -C BaseTools
    build -a ARM -p SamsungPlatformPkg/OrigenBoardPkg/OrigenBoardPkg-Exynos.dsc -t ARMLINUXGCC -b DEBUG

5\. Get the Linaro Linux distribution:

- Go on the Samsung Origen Linaro webpage:
  [https://releases.linaro.org/12.01/ubuntu/leb-origen/](https://releases.linaro.org/12.01/ubuntu/leb-origen/). Download the
  Origen HwPack:
  hwpack_linaro-leb-origen_20120123-1_armel_supported.tar.gz
- Download the minimal Linaro File System:
  [https://releases.linaro.org/12.01/ubuntu/oneiric-images/nano/linaro-o-nano-tar-20120123-1.tar.gz](https://releases.linaro.org/12.01/ubuntu/oneiric-images/nano/linaro-o-nano-tar-20120123-1.tar.gz)

6\. Install Linaro image Creator either from the tarball or the Ubuntu
package.

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

7\. Create the SD card. Replace the '/dev/sdX' with the device node
mapped to your MMC/SC card.

sudo linaro-media-create --mmc /dev/sdX --dev origen --binary linaro-o-nano-tar-20120123-1.tar.gz --hwpack
hwpack_linaro-leb-origen_20120123-1_armel_supported.tar.gz

8\. Replace u-boot by the EDK2 firmware you created in step 4:

    sudo dd if=Build/OrigenBoard-Exynos/DEBUG_ARMLINUXGCC/FV/ORIGENBOARD_EFI.fd of=/dev/sdb bs=512 seek=65

See this page [https://wiki.linaro.org/Boards/Origen/Setup](https://wiki.linaro.org/Boards/Origen/Setup) for more
information about the command.

9\. Insert the newly created SD card into the Origen SD card slot and
power up the platform.

    ```text
        UEFI firmware built at 19:29:37 on Feb 20 2012
        Timer 2,3 Configured
        UEFI firmware built at 19:29:38 on Feb 20 2012
    add-symbol-file
    /home/olivier/tianocore/Build/OrigenBoard-Exynos/DEBUG_ARMLINUXGCC/ARM/MdeModulePkg/Core/Dxe/DxeMain/DEBUG/DxeCore.dll
    0x4FD39240
        Loading DxeCore at 0x004FD39000 EntryPoint=0x004FD39241
    add-symbol-file
    /home/olivier/tianocore/Build/OrigenBoard-Exynos/DEBUG_ARMLINUXGCC/ARM/MdeModulePkg/Core/Dxe/DxeMain/DEBUG/DxeCore.dll
    0x4FD39004
        HOBLIST address in DXE = 0x4FD21590
        Memory Allocation 0x00000004 0x4FF9C000 - 0x4FFEBFFF
        Memory Allocation 0x00000004 0x4FF94000 - 0x4FF9BFFF
        Memory Allocation 0x00000004 0x4FFEC000 - 0x4FFFFFFF
        Memory Allocation 0x00000004 0x4FF84000 - 0x4FF93FFF
        (...)
    add-symbol-file
    /home/olivier/tianocore/Build/OrigenBoard-Exynos/DEBUG_ARMLINUXGCC/ARM/MdeModulePkg/Universal/Disk/UnicodeCollation/EnglishDxe/EnglishDxe/DEBUG/EnglishDxe.dll
    0x4F8B3240
        Loading driver at 0x0004F8B3000 EntryPoint=0x0004F8B3261 EnglishDxe.efi
        The default boot selection will start in   8 seconds
        [1] SD-MMC Booting
                - VenHw(B615F1F5-5088-43CD-809C-A16E52487D00)/HD(2,MBR,0x0009C1D9,0x2000,0x1A000)/uImage
                - Initrd: VenHw(B615F1F5-5088-43CD-809C-A16E52487D00)/HD(2,MBR,0x0009C1D9,0x2000,0x1A000)/uInitrd
                - Arguments: console=ttySAC2,115200n8  root=UUID=73ce15fe-4c31-47e5-b537-9759497e9615 rootwait ro
                - LoaderType: 1
        [2] EBL
        [3] Boot Manager
        Start:
    ```
