# ArmPlatformPkg ArmVExpressPkg

## Status

Build and Run on EDK2 Subversion revision 14897

- Requirements
  - RVCTv4 or ARM Compiler v5 (supplied in ARM DS-5) or ARMGCC
  - Using Ubuntu: gcc, make, uuid-dev
  - Using Cygwin: gcc, make, e2fsprogs (needed for uuid.h)

- Tested with:
  - Versatile Express DVD version 5.2 - [How to get the latest firmware
    for ARM Versatile
    Express?](https://arminfo.emea.arm.com/help/index.jsp?topic=/com.arm.doc.faqs/ka16308.html)
  - Linaro toolchain 2013.08 - [Get the latest
    version](https://launchpad.net/linaro-toolchain-binaries/+download)

- Serial Terminal settings
  - Baud Rates: 38400
  - Data: 8 bit
  - Parity: None
  - Flow Control: None

## Use ICE debugger with Versatile Express

Prior to use ICE debugger with Versatile Express, you will need to
update the version of the ICE's firmware. If you have not installed
RealView 4.0 SP3, do it. Open "RealView ICE Update" and "Install
Firmware Update ...". Install
"\[ARM_INSTALL_PATH\]\RVI\Firmware\3.4\11\ARM-RVI-3.4.0-25-base.rvi" and
"\[ARM_INSTALL_PATH\]\RVI\Firmware\3.4\22\ARM-RVI-3.4.59-59-patch.rvi".

## How to build UEFI Versatile Express

### For Linux

#### For the first time

1\. Get EDK2 from Tianocore Subversion repository

    svn co https://svn.code.sf.net/p/edk2/code/trunk/edk2 edk2 --username guest

2\. Set up the environment. And build the EDK2’s tools

    . edksetup.sh
    make -C BaseTools

3\. Ensure the GCC toolchain is in your PATH environment variable or
defined by the GCC47_ARM_PREFIX environment variable. Example:

    export GCC47_ARM_PREFIX=/opt/gcc-linaro-arm-linux-gnueabihf-4.8-2013.08_linux/bin/arm-linux-gnueabihf-

4\. Build the ARM Versatile Express UEFI Firmware

    build -a ARM -p ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-CTA9x4.dsc -t GCC47 -D EDK2_ARMVE_STANDALONE=0

5\. Edit the ARM Versatile Express configuration file images.txt to
declare the location of the UEFI firmware in NOR Flash

    TOTALIMAGES: 5                   ;Number of Images (Max : 32)

    NOR0UPDATE: AUTO                 ;Image Update:NONE/AUTO/FORCE
    NOR0ADDRESS: BOOT                ;Image Flash Address
    NOR0FILE: \SOFTWARE\bm_v209.axf  ;Image File Name

    NOR1UPDATE: AUTO                 ;IMAGE UPDATE:NONE/AUTO/FORCE
    NOR1ADDRESS: 44000000            ;Image Flash Address
    NOR1FILE: \SOFTWARE\sec_uefi.bin ;Image File Name
    NOR1LOAD: 0                      ;Image Load Address
    NOR1ENTRY: 0                     ;Image Entry Point

    NOR2UPDATE: AUTO                 ;IMAGE UPDATE:NONE/AUTO/FORCE
    NOR2ADDRESS: 45000000            ;Image Flash Address
    NOR2FILE: \SOFTWARE\uefi.bin     ;Image File Name
    NOR2LOAD: 45000000               ;Image Load Address
    NOR2ENTRY: 45000000              ;Image Entry Point

    NOR3UPDATE: AUTO                 ;IMAGE UPDATE:NONE/AUTO/FORCE
    NOR3ADDRESS: 46000000            ;Image Flash Address
    NOR3FILE: \SOFTWARE\kernel.bin   ;Image File Name
    NOR3LOAD: 46000000               ;Image Load Address
    NOR3ENTRY: 46000000              ;Image Entry Point

    NOR4UPDATE: AUTO                 ;IMAGE UPDATE:NONE/AUTO/FORCE
    NOR4ADDRESS: 40000000            ;Image Flash Address
    NOR4NAME: BOOTSCRIPT             ;Image Name
    NOR4FILE: \SOFTWARE\bootscr.txt  ;Image File Name

6\. To select second NOR Flash as a booting device, replace in the ARM
Versatile Express file \SITE1\HBI0191B\board.txt:

    SCC: 0x004 0x00001F09

By:

    SCC: 0x004 0x10001F09

7\. Copy
`Build/ArmVExpress-CTA9x4/DEBUG_GCC47/FV/SEC_ARMVEXPRESS_EFI.fd` to the
ARM Versatile Express mass storage (available when the board is
connected through USB to your host machine) under the folder SOFTWARE
and name sec_uefi.bin. Example for cygwin:

    cp Build/ArmVExpress-CTA9x4/DEBUG_GCC47/FV/SEC_ARMVEXPRESS_EFI.fd /cygdrive/e/SOFTWARE/sec_uefi.bin

8\. Start the ARM Versatile Express board. You should read
`Waiting for firmware at 0x80000000 ...` on the serial port.

9\. Copy ARMVEXPRESS_EFI.fd at 0x80000000 with RealView Debugger

    readfile,raw,nowarn "[EDK2_PATH]\Build\ArmVExpress-CTA9x4\DEBUG_GCC47\FV\ARMVEXPRESS_EFI.fd"=0x80000000

10\. Resume the execution from RealView Debugger

#### For all subsequent times

1\. Build ARM Versatile Express UEFI Firmware

    build -a ARM -p ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-CTA9x4.dsc -t GCC47 -D EDK2_ARMVE_STANDALONE=0

2\. Start the ARM Versatile Express board. You should read
`Waiting for firmware at 0x80000000 ...` on the serial port.

3\. Copy ARMVEXPRESS_EFI.fd at 0x80000000.

With RealView Debugger

    readfile,raw,nowarn "[EDK2_PATH]\Build\ArmVExpress-CTA9x4\DEBUG_GCC47\FV\ARMVEXPRESS_EFI.fd"=0x80000000

With ARM DS-5

    restore [EDK2_PATH]/Build/ArmVExpress-CTA9x4/DEBUG_GCC47/FV/ARMVEXPRESS_EFI.fd binary 0x80000000

**Note:** You can script this copy in DS-5 by adding the following
lines in the text box "Execute debugger commands" of the 'Debugger' tab
in "Debug Configurations":

    interrupt
    if $pc==0x80000000
      restore ~/tianocore/Build/ArmVExpress-CTA9x4/DEBUG_GCC47/FV/ARMVEXPRESS_EFI.fd binary 0x80000000
    end

4\. Resume the execution

### For RealView Compiler Toolchain on Windows

The command line window needs to be the one from Visual Studio to get
the environment variables required to get some development tools (the
windows compiler for BaseTools and \`nmake\`). The EDK2 toolchain name
for ARM RealView Compiler Toolchain under a Windows environment is
\`RVCT\`. The EDK2 build system will automatically pick up the RVCT
toolchain defined in your PATH. If you want to use a specific version,
set the environment variable 'RVCT_TOOLS_PATH':

    set RVCT_TOOLS_PATH=[YOUR_TOOLCHAIN_PATH]

### For RealView Compiler Toolchain on Linux

The EDK2 toolchain name for ARM RealView under a Linux environment is
\`RVCTLINUX\`. The EDK2 build system will automatically pick up the RVCT
toolchain defined in your PATH. If you want to use a specific version,
set the environment variable 'RVCT_TOOLS_PATH':

    export RVCT_TOOLS_PATH=[YOUR_TOOLCHAIN_PATH]

### For RVCT on Cygwin

The EDK2 toolchain name for ARM RealView under a Cygwin environment is
\`RVCTCYGWIN\`. The EDK2 build system will automatically pick up the
RVCT toolchain defined in your PATH. If you want to use a specific
version, set the environment variable 'RVCT_TOOLS_PATH':

    export RVCT_TOOLS_PATH=[YOUR_TOOLCHAIN_PATH]

### To support the standalone mode

The full ArmVe UEFI firmware can be written into NOR Flash to allow the
entire boot sequence to be done after a cold boot.

    build -a ARM -p ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-CTA9x4.dsc -t GCC47 -D EDK2_ARMVE_STANDALONE=1

ARMVEXPRESS_EFI.fd is required to be copied into the ARM Versatile
Express board:

    cp Build/ArmVExpress-CTA9x4-Standalone/DEBUG_GCC47/FV/SEC_ARMVEXPRESS_EFI.fd /cygdrive/e/SOFTWARE/sec_uefi.bin
    cp Build/ArmVExpress-CTA9x4-Standalone/DEBUG_GCC47/FV/ARMVEXPRESS_EFI.fd /cygdrive/e/SOFTWARE/uefi.bin

### Trustzone Support

ArmVE's UEFI supports booting Trustzone (two worlds: Secure and Normal
Worlds) and No Trustzone (one world: the CPU remains in Secure World)
supports. Trustzone support is enabled by Enabling SMC TZASC in the Test
Chip SCC Register 1. This register can only be changed by editing the
configuration file of your Versatile Express board:
E:\SITE1\HBI0191B\board.txt Changing: `SCC: 0x004 0x10001F09` For:
`SCC: 0x004 0x10003F09`

### Booting Linux

The default entry in the ARM Boot Manager is defined by the PCDs:

\- gArmPlatformTokenSpaceGuid.PcdDefaultBootDescription\|L"NorFlash"

\-
gArmPlatformTokenSpaceGuid.PcdDefaultBootDevicePath\|L"VenHw(E7223039-5836-41E1-B542-D7EC736C5E59)/MemoryMapped(0,0x46000000,0x462F0000)"

\- gArmPlatformTokenSpaceGuid.PcdDefaultBootArgument\|"root=/dev/sda2
rootwait debug earlyprintk console=ttyAMA0,38400 mem=1G"

\- gArmPlatformTokenSpaceGuid.PcdDefaultBootType\|1

The PCD PcdDefaultBootDevicePath expects a Device Path conforms to the
UEFI specification.
PcdDefaultBootType defines the type of the image pointed by
PcdDefaultBootDevicePath.

\- PcdDefaultBootDevicePath = 0 for an EFI Application

\- PcdDefaultBootDevicePath = 1 for a legacy kernel with ATAG support

\- PcdDefaultBootDevicePath = 2 for a kernel with Flat Device Tree (FDT)
support

Example of UEFI Device Path:

    // Load FDT binary from the Firmware Volume (mapped at 0x80000000)
    #define LINUX_KERNEL  L"MemoryMapped(11,0x80000000,0x6FEFFFFF)\\zImage.fdt"

    // Linux Kernel from a SD Card
    #define LINUX_KERNEL    L"VenHw(621B6FA5-4DC1-476F-B9D8-52C557D81070)/HD(1,MBR,0x00000000,0xF9,0x3C8907)\\boot\\zImage.fdt"

    // Kernel from SATA HD - Partition 2
#define LINUX_KERNEL
L"Acpi(PNP0A03,0)/Pci(0|0)/Pci(0|0)/Pci(5|0)/Pci(0|0)/Sata(0,0,0)/HD(2,MBR,0x00076730,0x1F21BF,0x1F21BF)\\boot\\zImage.fdt"

    // Kernel from NOR Flash
    #define LINUX_KERNEL            L"VenHw(02118005-9DA7-443a-92D5-781F022AEDBB)/MemoryMapped(0,0x46000000,0x462F0000)"

## Starting UEFI from ARM Boot Monitor on VExpress Core Tile A15x2-A7x3

1\. Build UEFI

    export EDK2_DSC=ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-CTA15-A7.dsc
    export EDK2_MACROS="-D ARM_BIGLITTLE_TC2=1"
    make -f ArmPlatformPkg/Scripts/Makefile

2\. Change the Boot core - BootMon use A15-0 as a primary core while
UEFI uses A7-0 Change from:

    SCC: 0x700 0x00320003           ;CFGRW48 - Boot cluster and CPU (CA15[0])

to:

    SCC: 0x700 0x10320003           ;CFGRW48 - Boot cluster and CPU (CA7[0])

3\. Configure the VExpress board with UEFI. Change the file
SITE1/HBI0249A/images.txt on the VExpress mass-storage such as (note:
the Boot Monitor image bm_v513r.axf might have a different name):

    TITLE: Versatile Express Images Configuration File

    [IMAGES]
    TOTALIMAGES: 4                   ;Number of Images (Max : 32)
    NOR0UPDATE: AUTO                 ;Image Update:NONE/AUTO/FORCE
    NOR0ADDRESS: BOOT                ;Image Flash Address
    NOR0FILE: \SOFTWARE\bm_v513r.axf ;Image File Name
    ;NOR0FILE: \SOFTWARE\bm_sram.axf ;Image File Name

    NOR1UPDATE: AUTO                 ;IMAGE UPDATE:NONE/AUTO/FORCE
    NOR1ADDRESS: 0x0d000000          ;Image Flash Address
    NOR1FILE: \SOFTWARE\uefi.bin     ;Image File Name
    NOR1LOAD: 0xB0000000
    NOR1ENTRY: 0xB0000000

    NOR2UPDATE: AUTO                 ;IMAGE UPDATE:NONE/AUTO/FORCE
    NOR2ADDRESS: 0x0e000000          ;Image Flash Address
    NOR2FILE: \SOFTWARE\zImage   ;Image File Name
    NOR2LOAD: 0
    NOR2ENTRY: 0

    NOR3UPDATE: AUTO                 ;IMAGE UPDATE:NONE/AUTO/FORCE
    NOR3ADDRESS: 0x0e800000          ;Image Flash Address
    NOR3FILE: \SOFTWARE\tc2.dtb   ;Image File Name
    NOR3LOAD: 0

4\. Start the board and type:

    flash run uefi

## Reseting the UEFI Variables stored in the Non Volatile storage on Versatile Express

The UEFI variables are stored in the NOR Flash of the Versatile Express.
The UEFI variables are stored on the smaller block size partition of the
NOR Flash - NOR Flash access is slow, using smaller block NOR Flash
allows to get better performance than using larger block.

To erase the UEFI variables in the Non-Volatile storage, you need to
restart the board to use ARM Boot Monitor (the ARM bootloader delivers
with the ARM Versatile Express). On some platforms, Boot Monitor is
enabled by changing the SCC used to switch NOR Flash mapping to boot
either Boot Monitor or UEFI.

At the Bootmon prompt:

    > flash
    Flash> list areas

    Base       Area Size Blocks Block Size
    ----       --------- ------ ----------
    0x40000000    65280K    255       256K
    0x43FC0000      256K      4        64K
    0x44000000    65280K    255       256K
    0x47FC0000      256K      4        64K

    Flash> erase range 0x43FC0000
    Erasing flash
    Flash> erase range 0x47FC0000
    Erasing flash
    Flash>

## ARM Fast Model

### Launching UEFI on the Model

Different ARM platform models have different parameters. To ensure the
parameters you are passing are supported by your model, list the
supported parameters by using the argument '-l'. Example:

    C:\Program Files\DS-5\bin>RTSM_VE_Cortex-A9_MPx4.exe -l
    # Parameters:
    # instance.parameter=value       #(type, mode) default = 'def value' : description : [min..max]
    #----------------------------------------------------------------------------------------------
    (...)
    motherboard.mmc.p_mmc_file="mmc.dat"                  # (string, init-time) default = 'mmc.dat' : MMCard filename
    motherboard.flashloader1.fname=""                     # (string, init-time) default = ''       : Filename
    motherboard.flashloader1.fnameWrite=""                # (string, init-time) default = ''       : FilenameWrite
    motherboard.flashloader0.fname=""                     # (string, init-time) default = ''       : Filename
    motherboard.flashloader0.fnameWrite=""                # (string, init-time) default = ''       : FilenameWrite
    (...)

The model supports various devices that UEFI already supports such as
the CLCD, the MMC controller, the NOR Flash, the RTC, etc

To start the UEFI in the model, ensure the UEFI firmware is loaded in
the NOR Flash mapped at 0x0 (generally NOR Flash 0). **Note:** At
reset, the CPU always starts at 0x0.

### Example: UEFI on RTSM Versatile Express Cortex A9x4

1\. Build UEFI

    . edksetup.sh
    build -a ARM -p ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-RTSM-A9x4.dsc -t RVCTLINUX

2\. Start the model with UEFI loaded in NOR Flash 0 (using Windows):

RTSM_VE_Cortex-A9_MPx4 -C motherboard.sp810_sysctrl.use_s8=1 -C
motherboard.flashloader0.fname=Build\ArmVExpress-RTSM-CTA9x4\DEBUG_RVCT\FV\ARMVEXPRESS_EFI.fd

Or start UEFI from the model Shell (using Linux):

model_shell -m RTSM_VE_Cortex-A9.so -C motherboard.sp810_sysctrl.use_s8=1 -C
motherboard.flashloader0.fname=Build/ArmVExpress-RTSM-CTA9x4/DEBUG_RVCTLINUX/FV/ARMVEXPRESS_EFI.fd

### Example: Run UEFI SCT on the Model

As said earlier, the model supports MMC, you could even run UEFI Self
Certifcation Test (SCT) on the Model: 1. Create a file that will contain
the SCT Binaries

    mkfs.vfat -C -n MMC_SD_CARD mmc.dat 131072

**Note:** 131072 is the size is in kilobytes (125M \* 1024)

2\. Mount the file in the loopback device /dev/loop0:

    mkdir fs
    sudo mount -o rw,loop=/dev/loop0,uid=`whoami`,gid=`whoami` mmc.dat fs

3\. Copy the SCT binaries (ArmPlatformSCT folder) on the mounted
filesystem (in the directory 'fs')

**Note:** If your platform firmware does not have EdkShell binary,
you could copy the binary on the filesysem. The EdkShell binary is
available in \[EDK2_ROOT\]/EdkShellBinPkg/FullShell/Arm/Shell_Full.efi

4\. Unmount the filesystem

    sudo umount fs

5\. Start the model with the location of your mmc.dat file with the
arguments: -C motherboard.mmc.p_mmc_file="mmc.dat"

6\. Start EdkShell and Install SCT (refer to SCT documentation for more
details).

### Example: Booting Linux on the Model

By default UEFI on RTSM will boot Linux from Semihosting. When using
Semihosting support, files are downloaded from the host machine. The
file name for the Linux kernel can be found in
ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-RTSM-A9x4.dsc:

    gArmPlatformTokenSpaceGuid.PcdDefaultBootDevicePath|L"VenHw(C5B9C74A-6D72-4719-99AB-C59F199091EB)/zImage"

A default Device Path may also be specified using the PCD
gArmPlatformTokenSpaceGuid.PcdFdtDevicePath. Example for a Device Tree
downloaded from Semihosting:

    gArmPlatformTokenSpaceGuid.PcdFdtDevicePath|L"VenHw(C5B9C74A-6D72-4719-99AB-C59F199091EB)/vexpress.dtb"

**Note:** The EFI Device Path Node
'VenHw(C5B9C74A-6D72-4719-99AB-C59F199091EB)' is Semihosting UEFI File
System driver (located at ArmPkg/Filesystem/SemihostFs). By default the
Semihosting support loads files from the location where the Fast Model
has been started. The Semihosting support does not support folder; all
files must be in the root of the default location.

### Example: Trustzone on the Fast Model

A ARM core with Security Extension (case of most ARMv7 core) always
starts in Secure World. But enabling Secure and Non-Secure worlds is not
enough to have a Secure Platform, you must have Secure and Non-Secure
memory. One way to have Secure and Non-Secure Memory regions is to use
Trustzone controllers (see:
[https://developer.arm.com/products/system-ip/system-controllers/other-system-controllers](https://developer.arm.com/products/system-ip/system-controllers/other-system-controllers)).
The Fast Model VExpress does not have Secure memory (this memory cannot
be read from Non-Secure world).

If you want to experiment Secure and Non-Secure worlds on the Fast
Model, you can use Non-Secure memory as Secure Memory. To enable both
worlds, you must set the PCD gArmTokenSpaceGuid.PcdTrustzoneSupport to
TRUE. ArmPlatformPkg/Sec module will be responsible to make the
transition from Secure to Non-Secure World.
