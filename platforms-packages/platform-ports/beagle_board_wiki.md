# Beagle Board Wiki

## Beagle Board

To find the latest instructions to build the BeagleBoard on Linux go to
[this
wikipage](beagle_board_pkg.md)

## Get the edk2 source tree and build BeagleBoardPkg in Windows® Cygwin Bash shell

The following instructions assume you have the ARM RealView Development
Suite v3.1 and Cygwin installed on Windows. Other versions of the
RealView Development Suite should work, but only v3.1 and v4.0 have been
tested.

If you use the command line version of subversion, then you can easily
checkout the edk2 and source to the FAT32 driver to the /cygdrive/c/edk2
directory with the following commands:

    /cygdrive/c$ svn co https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2 edk2 --username guest
    /cygdrive/c$ cd edk2
/cygdrive/c/edk2$ svn co https://edk2-fatdriver2.svn.sourceforge.net/svnroot/edk2-fatdriver2/trunk/FatPkg FatPkg
--username guest

Build the Beagle Board Package
Now you can run the build.sh script

    /cygdrive/c/edk2$ cd /cygdrive/c/edk2/BeagleBoardPkg
    /cygdrive/c/edk2/BeagleBoardPkg$ ./build.sh

As a tangible result of the build, the FLASH image for the Beagle Board
will end up in
/cygdrive/c/edk2/Build/BeagleBoa​rd/DEBUG_RVCT31CYGWI​N/FV/Beagle
Board_EFI​\_flashboot.fd.
Note: You may get a build error that looks like:

    /bin/sh: /cygdrive/c/Program Files/ARM/RVCT/Progr​ams/3.1/761/win_32-p​entium/armcc: No such file or directory

This means your ARM compiler is installed in a different location. You
will need to edit edk2/Conf/tools_def.txt to match the location your
compiler was installed (search for DEFINE RVCT31CYGWIN_TOOLS_PATH). The
tools_def.txt file is created when the ./build.sh script sources .
edksetup.sh BaseTools to setup the environment. It is copied from
edk2/BaseTools/Con​f/tools_def.template​ that is checked into source
control. You can make local edits to the tools_def.txt version and not
worry about accidentally checking it in to source control.

The ./build.sh script also supports arguments. If you pass RELEASE debug
code is stripped out. Please note the image that gets created is a fixed
size Firmware Volume, so setting RELEASE will increase the amount of
free space in the file system and not make the image physically smaller.
You can also do a clean by passing in clean. DEBUG (default for
./build.sh) and RELEASE builds are built in different directories. All
other arguments are passed directly to the edk2 build command. Here are
some examples:

    /cygdrive/c/edk2/BeagleBoardPkg$ ./build.sh clean
    /cygdrive/c/edk2/BeagleBoardPkg$ ./build.sh RELEASE
    /cygdrive/c/edk2/BeagleBoardPkg$ ./build.sh RELEASE clean
    /cygdrive/c/edk2/BeagleBoardPkg$ ./build.sh -y report.log -v

## Get the edk2 source tree and build BeagleBoardPkg Visual Studio .NET 2003 Command Prompt Window

The following instructions assume you have the ARM RealView Development
Suite v3.1 and Subversion installed on Windows. Other versions of the
RealView Development Suite should work, but only v3.1 and v4.0 have been
tested.

If you don't have Cygwin installed you can't use the build.sh Bash
script in the BeagleBoardPkg directory. Building on Windows is a little
simpler as binary versions of all the tools are checked in.

If you use the command line version of subversion, then you can easily
checkout the edk2 to the C:\edk2 directory with the following command:

    C:\> svn co     https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2 C:\edk2 --username guest
    C:\> cd C:\edk2
    C:\edk2> svn co https://edk2-fatdriver2.svn.sourceforge.net/svnroot/edk2-fatdriver2/trunk/FatPkg FatPkg --username guest

The b.bat script builds the EFI Beagle Board image, patches the
beginning of the image with information needed by the mask ROM, and
builds some debug scripts for RealView and Trace32 JTAG debuggers.

    C:\> cd C:\edk2\BeagleBoardPkg
    C:\edk2\BeagleBoardPkg> b

The b.bat script also supports arguments. If you pass RELEASE debug code
is stripped out. Please note the image that gets created is a fixed size
Firmware Volume, so setting RELEASE will increase the amount of free
space in the file system and not make the image physically smaller. You
can also do a clean by passing in clean. DEBUG (default for b.bat) and
RELEASE builds are built in different directories. All other arguments
are passed directly to the edk2 build command. Here are some examples:

    C:\edk2\BeagleBoardPkg> b clean
    C:\edk2\BeagleBoardPkg> b RELEASE
    C:\edk2\BeagleBoardPkg> b RELEASE clean
    C:\edk2\BeagleBoardPkg> b -y report.log -v

Note: You may get a build error that looks like:

    NMAKE : fatal error U1077: '"c:/Program Files/ARM/RVCT/Programs/3.1/761/win_32-pentium/armcc"' : return code '0x1'

This means your ARM compiler is installed in a different location. You
will need to edit edk2\Conf\tools_def.txt to match the location your
compiler was installed (search for DEFINE RVCT31_TOOLS_PATH). The
tools_def.txt file is created when the edksetup.bat script ran to setup
the environment. It is copied from
edk2\BaseTools\Con​f\tools_def.template​ that is checked into source
control. You can make local edits to the .txt version and not worry
about accidentally checking it in to source control.

## Boot EFI from RAM on Beagle Board using RealView Debugger

If you have an RealView Debugger hooked up to your Beagle Board you can
use /cygdrive/c/edk2/Build/BeagleBoa​rd/DEBUG_RVCT31CYGWI​
N/rvi_boot_from_ram.​inc to down load the EFI image over JTAG and boot
it. To load the script go to the Tools menu and select Include Commands
from File... You can use
edk2/Build/BeagleBoa​rd/DEBUG_RVCT31CYGWI​N/rvi_load_symbols.i​nc to load
symbols for the multiple EFI images in the debugger. Note: Some early
versions of the RVI have a bug as the script can not access memory and
does not work. You need to load the symbols after you break into the
debugger.

When the Beagle Board boots from the NAND FLASH the mask ROM on the
Beagle Board executes commands out of the start of the NAND to turn on
memory and then copies the image from the NAND into system memory and
jumps to it. In the EFI world this NAND image is called an FD (Flash
Device) image and it contains 520 bytes of image header for the mask ROM
and an FV (Firmware Volume). The FV is a simple FLASH file system and
the first 4 bytes of the FV contain a jump to the SEC (SEcuirty Core)
module. The SEC is a PE/COFF image that contains the reset vector and it
is located in an arbitrary location in the FV. When you boot from JTAG
the mask ROM on the Beagle Board does not run and the RVD script copies
the FD into system memory and sets the PC to first location in the FV.

If you have the version of the debugger that does not support the
rvi_load_symbols.inc script you can do the following to load symbols.
Edit the following lines of
/cygdrive/c/edk2/BeagleBoardPkg/BeagleBoardPkg.dsc:

    #  PeCoffExtraActionLib|ArmPkg/Library/RvdPeCoffExtraActionLib/RvdPeCoffExtraActionLib.inf
      PeCoffExtraActionLib|MdePkg/Library/BasePeCoffExtraActionLibNull/BasePeCoffExtraActionLibNull.inf

and convert them to:

      PeCoffExtraActionLib|ArmPkg/Library/RvdPeCoffExtraActionLib/RvdPeCoffExtraActionLib.inf
    #  PeCoffExtraActionLib|MdePkg/Library/BasePeCoffExtraActionLibNull/BasePeCoffExtraActionLibNull.inf

This will send a series of commands to the RVD console, via semihosting,
that can be used to load symbols. Please note that sending all this data
via semihosting slows boot down a lot. After the system gets to the
point where you would like to source level debug stop execution on the
target. Copy the contents of the StdIO window to a file, and use Tools
Include Commands from File... You should now have source level debug for
any EFI module that was loaded in memory.
The reset vector code exists in the SEC module located in the FV. The
SEC was relocated to its execution address by the build tool that
constructed the FV. Since this code runs from a fixed address you have
to manually load symbols for this code. The SEC loads the DXE Core and
that should be the first load command you see in the StdIO window of the
debugger. The following is an example of the first prints that come out
of the SEC code. You can cut the load command and past it directly into
the RVD Cmd window to source level debug the SEC.

    UART Enabled
load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31CYGWIN\ARM\BeagleBoardPkg\Sec\Sec\DEBUG\BeagleBoardSec.dll
&0x80008360

## Put edk2 code in the Beagle Board NAND using U-Boot

The OMAP mask ROM can boot from the MMC/SD card regardless of the state
of the NAND (normal boot location). To boot from an SD card it has to be
constructed as detailed in Step \#1. You then power cycle the Beagle
Board (unplug it and plug it back in) while holding down the USER
button.

Step \#1:
Follow the
[instructions](https://code.google.com/p/beagleboard/wiki/BeagleboardRevCValidation)
and build an MMC/SD card that boots the Beagle Board U-Boot.

You only need to do the following steps:
Prepare MMC/SD card for Validation
Copy the following files on to MMC in the following order:
When copying files to the SD card make sure you follow the NOTE and copy
"Regular script file" as boot.scr.
In addition to the above instructions copy
edk2\Build\BeagleB​oard\DEBUG_RVCT31CY​GWIN\FV\BeagleBoar​
d_EFI_flashboot.fd to MMC/SD card.

Step \#2
Place SD card back in Beagle Board
power cycle while holding down USER button
hit a key on the serial console to stop u-boot from loading Linux

Step \#3:
At the U-Boot prompt (currently OMAP3 beagleboard.org \# ) type the
following commands to put the EFI code in the NAND:

    OMAP3 beagleboard.org # mmcinit
    OMAP3 beagleboard.org # fatload mmc 0 80208000 BeagleBoard_EFI_flashboot.fd
    OMAP3 beagleboard.org # nandecc hw
    OMAP3 beagleboard.org # nand erase 0 80000
    OMAP3 beagleboard.org # nand write 80208000 0 80000

Step \#4
Hit the reset button and you should see DEBUG prints from EFI. You
should get to the prompt and it will look like:

    Embedded Boot Loader (EBL) prototype. Built at 16:18:20 on Dec 9 2009
    THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN 'AS IS' BASIS,
    WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR IMPLIED.
    Please send feedback to dev@edk2.tianocore.org
    BeagleEdk2>

This works for getting the EFI image in the NAND the 1st time, and is
also a way to recover the image if the NAND ever gets into a bad state.

## Put edk2 code in the Beagle Board DRAM using U-Boot

See [Booting
EDK2 in the Beagle Board DRAM using U-Boot](beagle_board_pkg.md#booting-edk2-in-the-beagle-board-dram-using-u-boot)

## Put edk2 code in the Beagle Board NAND using EBL

**Step \#1:**
Copy edk2\Build\BeagleB​oard\DEBUG_RVCT31CY​GWIN\FV\BeagleBoar​
d_EFI_flashboot.fd to a MMC/SD card

**Step \#2:**
Place SD card back in Beagle Board
Boot EFI on the Beagle Board (power cycle if you have it in NAND for
example)

**Step \#3:**
Use the EFI EBL to flash the image

    BeagleEdk2> cp fs1:\Beagle Board_EFI_flashboot.fd blk0:

## Beagle Board Boot Flow

The Beagle Board does not implement PEI. SEC loads the DXE core
directly. See the
[PI Boot
Flow](../../development/design-architecture/pi_boot_flow.md) for an overview of PI booting.

**NAND FLASH Layout**
The OMAP3530 used on the Beagle Board contains a mask ROM that initiates
the boot process. The image in the NAND FLASH starts with a 512 (0x200)
byte Table of Contents (TOC) and a set of configuration headers. The
mask ROM uses information in the configuration headers to program clocks
and turn on DRAM. After DRAM is initialized the Initial software is
copied from NAND to DRAM. The first 4 bytes of the image contain the
size of the image, and the next 4 bytes contain the address of where the
image should be copied. On the Beagle Board the image is copied to
0x80008208.
![](OMAP_ROM_Boot.key.jpg)
The values used to initialize clocks and DRAM come from the
[ConfigurationHeader.dat](https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2/BeagleBoardPkg/ConfigurationHeader.dat)
text file. The build process uses the
[GenerateImage](https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2/BeagleBoardPkg/Tools/generate_image.c)
utility to parse the ConfigurationHeader.dat file and patch the
information required by the OMPA3530 mask ROM into the beginning of the
image.

The edk2 build process creates a Firmware Device (FD) image that has
space reserved for the OMAP3530 configuration headers. In the edk2 the
FD layout is controlled by a Flash Description File (.fdf). The Flash
Description file for the Beagle Board is called
[BeagleBoardPkg.fdf](https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2/BeagleBoardPkg/BeagleBoardPkg.fdf).
The Beagle Board FD contains 512 (0x200) bytes of OMAP3530 configuration
headers followed by 8 bytes of the OMAP3530 image header. The OMAP3530
image header contains the size of the Software Image and the location
the image should be loaded in DRAM. For the Beagle Board the Software
Image is a Firmware Volume (FV) called FVMAIN_COMPACT. FVMAIN_COMPACT
contains two files. The first file is the SEC code that the mask ROM
jumps into and the 2nd file is a compressed FV, called FVMAIN.
FVMAIN_COMPACT is called fv0 in the EBL shell. You can use the dir
command to see the contents of the FV.

    BeagleEdk2>dir fv0:
       19,800     SEC D959E387-7B91-452C-90E0-A1DBAC90DDB8
       99,271      FV 9E21FD93-9C72-4C15-8C4B-E77F1DB2D792
                 119,071 bytes in files 404,569 bytes free

FVMAIN is called fv1 and this is the decompressed copy of the FV file in
fv0:

    BeagleEdk2>dir fv1:
       44,568 DxeCore D6A2CB7F-6A18-4E2F-B43B-9920A733700A DxeCore
       22,100  Driver B8D9777E-D72A-451F-9BDB-BAFB52A68415 ArmCpuDxe
        5,062  Driver B601F8C4-43B7-4784-95B1-F4226CB40CEE RuntimeDxe
        2,480  Driver F80697E9-7FD6-4665-8646-88E33EF71DFC SecurityStubDxe
        1,788  Driver F099D67F-71AE-4C36-B2A3-DCEB0EB2B7D8 WatchdogTimer
        3,780  Driver 42857F0A-13F2-4B21-8A23-53D3F714B840 CapsuleRuntimeDxe
        6,396  Driver 02B01AD5-7E59-43E8-A6D8-238180613A5A EmuVariableRuntimeDxe
        1,378  Driver FCABE6A7-7953-4A84-B7EC-D29E89B62E87 EmbeddedMonotonicCounter
        2,492  Driver 6696936D-3637-467C-87CB-14EA8248948C SimpleTextInOutSerial
        2,236  Driver 16036A73-E8EF-46D0-953C-9B8E96527D13 Reset
        1,260  Driver B336F62D-4135-4A55-AE4E-4971BBF0885D RealTimeClock
        1,834  Driver 4C6E0267-C77D-410D-8100-1495911A989D MetronomeDxe
        3,102  Driver C5B9C74A-6D72-4719-99AB-C59F199091EB SemihostFs
        4,452  Driver 4D00EF14-C4E0-426B-81B7-30A00A14AAD6 NandFlash
        4,364  Driver 100C2CFA-B586-4198-9B4C-1683D195B1DA MMCHS
        1,724  Driver D5125E0F-1226-444F-A218-0085996ED5DA Smbus
        1,402  Driver E7D9CAE1-6930-46E3-BDF9-0027446E7DF2 Gpio
        2,144  Driver 23EED05D-1B93-4A1A-8E1B-931D69E37952 BeagleBoardInterruptDxe
        2,728  Driver 6DDBF08B-CFC9-43CC-9E81-0784BA312CA0 BeagleBoardTimerDxe
        1,490  Driver 71FE861A-5450-48B6-BFB0-B93522616F99 TPS65950
        4,156  Driver 6B38F7B4-AD98-40E9-9093-ACA2B5A253C4 DiskIoDxe
        8,898  Driver 1FA1F39E-FEFF-4AAE-BD7B-38A070A3B609 PartitionDxe
       14,992  Driver 961578FE-B6B7-44C3-AF35-6BC705CD2B1F Fat
        2,686  Driver CD3BAFB6-50FB-4FE8-8E4E-AB74D2C1A600 EnglishDxe
        3,892  Driver FEAA2E2B-53AC-4D5E-AE10-1EFD5DA4A2BA BeagleBoardPciEmulation
       12,056  Driver BDFE430E-8F2A-4DB0-9991-6F856594777E EhciDxe
       11,484  Driver 240612B7-A063-11D4-9A3A-0090273FC14D UsbBusDxe
        8,268  Driver 9FB4B4A7-42C0-4BCD-8540-9BCC6711F83E UsbMassStorageDxe
       44,016     App 3CEF354A-3B7A-4519-AD70-72A134698311 Ebl
        7,246  Driver 934431FE-5745-402E-913D-17B4434EB0F3 BeagleBoardBds
                 234,474 bytes in files 846 bytes free

The edk2 build process places all the .fd and .fv files in the FV
directory of the build root. The build root starts in the edk2 directory
as Build/BeagleBoard then there is a directory name that contains the
Target (DEBUG or RELEASE build), an underscore, and then the target
tools type. So for example edk2\Build\BeagleBoard\DEBUG_RVCT31\FV would
be the DEBUG build using RVCT 3.1 compiler.

{\| border="1" \|+ Files Related to the Beagle Board NAND FLASH Image !
File !! Description \|- ! BeagleBoard_EFI_flashboot.fd \| Final image
that can be placed in FLASH or copied to DRAM @ 0x80008000 \|- !
BEAGLEBOARD_EFI.fd \| Contains FVs, but has not been patched with
OMAP3530 configuration headers \|- ! FVMAIN_COMPACT.Fv \| The Firmware
Volume in BEAGLEBOARD_EFI.fd that contains the SEC (reset vector code)
and a compressed copy of FVMAIN.Fv \|- ! FVMAIN.Fv \| Fimrware Volume
containing the EFI drivers used for booting \|}
If you have your shell set up to build the edk2 the VolInfo command
should be in your path and you can use it to dump out the contents of a
.Fv file.

**Reset Vector**
The OMAP 3530 mask ROM executes first and used the configuration headers
found in [NAND FLASH Layout](#NAND_FLASH_Layout) to turn on DRAM and
shadow the NAND FLASH image into memory at 0x80008208. Address 0x8008208
is the beginning of the FVMAIN_COMPACT.Fv. An FV starts with a
EFI_FIRMWARE_VOLUME_HEADER data structure. On the Beagle Board the first
4 bytes of the EFI_FIRMWARE_VOLUME_HEADER.ZeroVector has been patched by
a build tool to be a branch to the entry point of the SEC PE/COFF Image.

    ///
    /// Describes the features and layout of the firmware volume.
    ///
    typedef struct {
      ///
      /// The first 16 bytes are reserved to allow for the reset vector of
      /// processors whose reset vector is at address 0.
      ///
      UINT8                     ZeroVector[16];
      ///
      /// Declares the file system with which the firmware volume is formatted.
      ///
      EFI_GUID                  FileSystemGuid;
      ///
      /// Length in bytes of the complete firmware volume, including the header.
      ///
      UINT64                    FvLength;
      ///
      /// Set to EFI_FVH_SIGNATURE
      ///
      UINT32                    Signature;
      ///
      /// Declares capabilities and power-on defaults for the firmware volume.
      ///
      EFI_FVB_ATTRIBUTES_2      Attributes;
      ///
      /// Length in bytes of the complete firmware volume header.
      ///
      UINT16                    HeaderLength;
      ///
      /// A 16-bit checksum of the firmware volume header. A valid header sums to zero.
      ///
      UINT16                    Checksum;
      ///
      /// Offset, relative to the start of the header, of the extended header
      /// (EFI_FIRMWARE_VOLUME_EXT_HEADER) or zero if there is no extended header.
      ///
      UINT16                    ExtHeaderOffset;
      ///
      /// This field must always be set to zero.
      ///
      UINT8                     Reserved[1];
      ///
      /// Set to 2. Future versions of this specification may define new header fields and will
      /// increment the Revision field accordingly.
      ///
      UINT8                     Revision;
      ///
      /// An array of run-length encoded FvBlockMapEntry structures. The array is
      /// terminated with an entry of {0,0}.
      ///
      EFI_FV_BLOCK_MAP_ENTRY    BlockMap[1];
    } EFI_FIRMWARE_VOLUME_HEADER;

    typedef struct {
      ///
      /// The number of sequential blocks which are of the same size.
      ///
      UINT32 NumBlocks;
      ///
      /// The size of the blocks.
      ///
      UINT32 Length;
    } EFI_FV_BLOCK_MAP_ENTRY;

The first line of edk2 source code that is executed is the symbol
\_ModuleEntryPoint in the SEC. The edk2 has ARMASM compatible assembler
files that have a .asm extension, and gcc compatible assembler files
that have a .S extension.
[ModuleEntryPoint.asm](https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2/BeagleBoardPkg/Sec/Arm/ModuleEntryPoint.asm)
and
[ModuleEntryPoint.S](https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2/BeagleBoardPkg/Sec/Arm/ModuleEntryPoint.S)
contain \_ModuleEntryPoint.
The primary job of the SEC is to load the DXE Core. The DXE Core can be
thought of as the EFI mini-kernel, as it produces the EFI boot and
runtime services as called out in the UEFI specification. The Beagle
Board SEC does the following:

- Disable L2
- Enable Alignment checking
- Set CPU vectors to start of DRAM 0x80000000
- Switch to SVC mode, and set up a single stack
- Configure pads for input/output
- Initialize Clocks
- Enable Branch Prediction
- Turn on MMU and caches
- Initialize UART
- Build HOBs needed by DXE Core
- Start free running timer for the timer lib
- Decompress compressed FV
- Boot DXE Core

**DXE Dispatch**

The DXE Core is for the most part generic code. The DXE core dispatches
drivers that add APIs called Protocols to the system. Dispatching
involves choosing the order that drivers are loaded. The drivers are
PE/COFF images and they are loaded an relocated into free memory, and
then control is passed to the driver to run its initialization function.
The drivers initialization function does work, registers protocols, and
returns to the DXE core so the next driver can be dispatched. The UEFI
PI specification introduces the concept of a dependency grammar. The
dependency grammar allows a driver to run before or after another driver
(not common) or wait for the protocols the driver depends on to be
present in the system. The dependency grammar is stack based and
contains the following opcodes:

- BEFORE `<File Name GUID>`
- AFTER `<File Name GUID>`
- PUSH
- AND
- OR
- NOT
- TRUE
- FALSE
- END
- SOR (Schedule on Request)

A file in an FV consists of multiple sections and a driver will contain
a PE/COFF section and an optional dependency section. The dependency
section contains the byte codes of the dependency grammar. However, the
dependency sections are generated from the \[Depex\] section of the
drivers .inf file. The .inf file is the build control file for the
driver.
[TimerDxe.inf](https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2/Omap35xxPkg/TimerDxe/TimerDxe.inf)
is an example of an .inf file for a driver that depends on the
gHardwareInterruptProtocolGuid to be present before it gets
dispatched.
There are four common dependency expression forms that are used:

- TRUE
- gHardwareInterruptProtocolGuid
- gHardwareInterruptProtocolGuid AND gSecondProtocolGuid
- No Dependency expression

A Dependency expression of TRUE means the driver can dispatched at any
time. In practice the DXE core will dispatch any driver who's dependency
expression evaluates to true. If multiple drivers evaluate to true the
drivers will be loaded in an arbitrary order, usually the order they
were found in the FV. Usually drivers will be coded so they depend on
one or more protocol being present and the Depex section in the .inf
file will contain a set of protocols that are AND'ed together.
A driver that contains no dependency expression is a special case. It is
not the same as a dependency expression of TRUE. No dependency
expression implies that the driver is an EFI driver and depends on all
EFI services being present. The PI specification defines protocols,
named architectural protocols, that are required by the DXE core to
implement the boot and runtime services defined in the UEFI
specification. So no dependency expression implies a dependency
expression of all the following protocols AND'ed together:

- gEfiSecurityArchProtocolGuid
- gEfiCpuArchProtocolGuid
- gEfiMetronomeArchProtocolGuid
- gEfiTimerArchProtocolGuid
- gEfiBdsArchProtocolGuid
- gEfiWatchdogTimerArchProtocolGuid
- gEfiRuntimeArchProtocolGuid
- gEfiVariableArchProtocolGuid
- gEfiVariableWriteArchProtocolGuid
- gEfiCapsuleArchProtocolGuid
- gEfiMonotonicCounterArchProtocolGuid
- gEfiResetArchProtocolGuid
- gEfiRealTimeClockArchProtocolGuid

The debug prints from a DEBUG build of the Beagle Board show the
dispatch order on the Beagle Board:

    Loading driver at 0x00087EDA000 EntryPoint=0x00087EDC5F5 RuntimeDxe.efi
    Loading driver at 0x00087EB3000 EntryPoint=0x00087EB5081 SecurityStubDxe.efi
    Loading driver at 0x00087ED5000 EntryPoint=0x00087ED8F29 EmuVariableRuntimeDxe.efi
    Loading driver at 0x00087ED3000 EntryPoint=0x00087ED482D EmbeddedMonotonicCounter.efi
    Loading driver at 0x00087EB0000 EntryPoint=0x00087EB1D21 SimpleTextInOutSerial.efi
    Loading driver at 0x00087EAD000 EntryPoint=0x00087EAED8D Reset.efi
    Loading driver at 0x00087EAB000 EntryPoint=0x00087EAC6DD RealTimeClock.efi
    Loading driver at 0x00087EA9000 EntryPoint=0x00087EAA941 MetronomeDxe.efi
    Loading driver at 0x00087EA6000 EntryPoint=0x00087EA857D NandFlash.efi
    Loading driver at 0x00087EA3000 EntryPoint=0x00087EA4AB1 Smbus.efi
    Loading driver at 0x00087EA1000 EntryPoint=0x00087EA28CD Gpio.efi
    Loading driver at 0x00087E9E000 EntryPoint=0x00087E9FDC5 BeagleBoardInterruptDxe.efi
    Loading driver at 0x00087E9A000 EntryPoint=0x00087E9CE3D BeagleBoardBds.efi
    Loading driver at 0x00087E92000 EntryPoint=0x00087E97C29 ArmCpuDxe.efi
    Loading driver at 0x00087ED0000 EntryPoint=0x00087ED2109 CapsuleRuntimeDxe.efi
    Loading driver at 0x00087E8F000 EntryPoint=0x00087E90C45 BeagleBoardTimerDxe.efi
    Loading driver at 0x00087E8C000 EntryPoint=0x00087E8DA11 TPS65950.efi
    Loading driver at 0x00087E89000 EntryPoint=0x00087E8A985 WatchdogTimer.efi
    Loading driver at 0x00087E85000 EntryPoint=0x00087E87CC5 MMCHS.efi
    Loading driver at 0x00087E81000 EntryPoint=0x00087E841B5 BeagleBoardPciEmulation.efi
    Loading driver at 0x00087E7D000 EntryPoint=0x00087E7FB1D SemihostFs.efi
    Loading driver at 0x00087E7A000 EntryPoint=0x00087E7C531 DiskIoDxe.efi
    Loading driver at 0x00087E75000 EntryPoint=0x00087E79599 PartitionDxe.efi
    Loading driver at 0x00087E6C000 EntryPoint=0x00087E737B5 Fat.efi
    Loading driver at 0x00087E69000 EntryPoint=0x00087E6A9E9 EnglishDxe.efi
    Loading driver at 0x00087E61000 EntryPoint=0x00087E67CD5 EhciDxe.efi
    Loading driver at 0x00087E58000 EntryPoint=0x00087E5F529 UsbBusDxe.efi
    Loading driver at 0x00087E52000 EntryPoint=0x00087E56539 UsbMassStorageDxe.efi

## How do I load symbols for debugging

The [Beagle Board Boot Flow](#Beagle_Board_Boot_Flow) section
describes how UEFI, and especially PI, boot by loading a series of
PE/COFF images. A natural question to ask is how load symbols for a
debugger, especially when you consider that EFI defaults to load images
at arbitrary addresses. Luckily the UEFI specification has a concept
know as an EFI Debug Support Table that is defined in section 17.4 of
UEFI 2.3 specification. Using the EFI Debug Support Table it is possible
to write a debugger script to load symbols.
As an example of how to utilize the EFI debug Support Table the Beagle
Board port has an extra EBL shell command to dump the symbol table
information contained in the EFI Debug Support Table. This extra EBL
shell command is called symboltable and the source code can be found in
[EblCmdLib.c](https://edk2.svn.sourceforge.net/svnroot/edk2/trunk/edk2/BeagleBoardPkg/Library/EblCmdLib/EblCmdLib.c).
The symboltable (sync commands can be shortened as long as they are
unique, we will use sym as a synonym for symboltable) command parses the
EFI Debug Support Table and dumps out a set of commands that can be used
with a debugger. The default for sym is to dump out RealView Debugger
symbol load commands. The following is an example from a Beagle Board.

    BeagleEdk2>sym
load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Core\Dxe\DxeMain\DEBUG\DxeCore.dll &
0x87F2F240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Core\RuntimeDxe\RuntimeDxe\DEBUG\RuntimeDxe.dll &
0x87EDA240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\SecurityStubDxe\SecurityStubDxe\DEBUG\SecurityStubDxe.dll
& 0x87EB3240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Variable\EmuRuntimeDxe\EmuVariableRuntimeDxe\DEBUG\EmuVariableRuntimeDxe.dll
& 0x87ED5240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\EmbeddedMonotonicCounter\EmbeddedMonotonicCounter\DEBUG\EmbeddedMonotonicCounter.dll
& 0x87ED3240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\SimpleTextInOutSerial\SimpleTextInOutSerial\DEBUG\SimpleTextInOutSerial.dll
& 0x87EB0240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\ResetRuntimeDxe\ResetRuntimeDxe\DEBUG\Reset.dll & 0x87EAD240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\RealTimeClockRuntimeDxe\RealTimeClockRuntimeDxe\DEBUG\RealTimeClock.dll
& 0x87EAB240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\MetronomeDxe\MetronomeDxe\DEBUG\MetronomeDxe.dll &
0x87EA9240
    load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\Flash\Flash\DEBUG\NandFlash.dll & 0x87EA6240
    load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\SmbusDxe\Smbus\DEBUG\Smbus.dll & 0x87EA3240
    load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\Gpio\Gpio\DEBUG\Gpio.dll & 0x87EA1240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\InterruptDxe\InterruptDxe\DEBUG\BeagleBoardInterruptDxe.dll
& 0x87E9E240
load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\BeagleBoardPkg\Bds\Bds\DEBUG\BeagleBoardBds.dll &
0x87E9A240
load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\ArmPkg\Drivers\CpuDxe\CpuDxe\DEBUG\ArmCpuDxe.dll &
0x87E92240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\CapsuleRuntimeDxe\CapsuleRuntimeDxe\DEBUG\CapsuleRuntimeDxe.dll
& 0x87ED0240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\TimerDxe\TimerDxe\DEBUG\BeagleBoardTimerDxe.dll & 0x87E8F240
load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\TPS65950Dxe\TPS65950\DEBUG\TPS65950.dll &
0x87E8C240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\WatchdogTimerDxe\WatchdogTimer\DEBUG\WatchdogTimer.dll
& 0x87E89240
    load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\MMCHSDxe\MMCHS\DEBUG\MMCHS.dll & 0x87E85240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\PciEmulation\PciEmulation\DEBUG\BeagleBoardPciEmulation.dll
& 0x87E81240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\ArmPkg\Filesystem\SemihostFs\SemihostFs\DEBUG\SemihostFs.dll &
0x87E7D240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Disk\DiskIoDxe\DiskIoDxe\DEBUG\DiskIoDxe.dll &
0x87E7A240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Disk\PartitionDxe\PartitionDxe\DEBUG\PartitionDxe.dll
& 0x87E75240
    load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\FatPkg\EnhancedFatDxe\Fat\DEBUG\Fat.dll & 0x87E6C240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Disk\UnicodeCollation\EnglishDxe\EnglishDxe\DEBUG\EnglishDxe.dll
& 0x87E69240
load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Bus\Pci\EhciDxe\EhciDxe\DEBUG\EhciDxe.dll &
0x87E61240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Bus\Usb\UsbBusDxe\UsbBusDxe\DEBUG\UsbBusDxe.dll &
0x87E58240
load /a /ni /np
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Bus\Usb\UsbMassStorageDxe\UsbMassStorageDxe\DEBUG\UsbMassStorageDxe.dll
& 0x87E52240
    load /a /ni /np c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\Ebl\Ebl\DEBUG\Ebl.dll & 0x87160240

The 1st argument to sym is optional and it can be used to change the
template for how the symbol file load commands are printed out. The
default string is for the RealView Debugger. The first format specifier
in the string must be for an ASCII string for the file name of the
symbol file and the second format specifier must be for the load address
of the PE/COFF image. The EFI Print routine is not fully compatible with
POSIX printf. %a is for an ASCII string, %s is for Unicode, and %x is
for a hex number with no leading 0x.

    BeagleEdk2>sym "add-symbol-file %a 0x%x"
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Core\Dxe\DxeMain\DEBUG\DxeCore.dll
0x87F2F240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Core\RuntimeDxe\RuntimeDxe\DEBUG\RuntimeDxe.dll 0x87EDA240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\SecurityStubDxe\SecurityStubDxe\DEBUG\SecurityStubDxe.dll
0x87EB3240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Variable\EmuRuntimeDxe\EmuVariableRuntimeDxe\DEBUG\EmuVariableRuntimeDxe.dll
0x87ED5240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\EmbeddedMonotonicCounter\EmbeddedMonotonicCounter\DEBUG\EmbeddedMonotonicCounter.dll
0x87ED3240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\SimpleTextInOutSerial\SimpleTextInOutSerial\DEBUG\SimpleTextInOutSerial.dll
0x87EB0240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\ResetRuntimeDxe\ResetRuntimeDxe\DEBUG\Reset.dll 0x87EAD240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\RealTimeClockRuntimeDxe\RealTimeClockRuntimeDxe\DEBUG\RealTimeClock.dll
0x87EAB240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\MetronomeDxe\MetronomeDxe\DEBUG\MetronomeDxe.dll 0x87EA9240
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\Flash\Flash\DEBUG\NandFlash.dll 0x87EA6240
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\SmbusDxe\Smbus\DEBUG\Smbus.dll 0x87EA3240
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\Gpio\Gpio\DEBUG\Gpio.dll 0x87EA1240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\InterruptDxe\InterruptDxe\DEBUG\BeagleBoardInterruptDxe.dll
0x87E9E240
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\BeagleBoardPkg\Bds\Bds\DEBUG\BeagleBoardBds.dll
0x87E9A240
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\ArmPkg\Drivers\CpuDxe\CpuDxe\DEBUG\ArmCpuDxe.dll
0x87E92240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\CapsuleRuntimeDxe\CapsuleRuntimeDxe\DEBUG\CapsuleRuntimeDxe.dll
0x87ED0240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\TimerDxe\TimerDxe\DEBUG\BeagleBoardTimerDxe.dll 0x87E8F240
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\TPS65950Dxe\TPS65950\DEBUG\TPS65950.dll
0x87E8C240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\WatchdogTimerDxe\WatchdogTimer\DEBUG\WatchdogTimer.dll
0x87E89240
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\MMCHSDxe\MMCHS\DEBUG\MMCHS.dll 0x87E85240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\PciEmulation\PciEmulation\DEBUG\BeagleBoardPciEmulation.dll
0x87E81240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\ArmPkg\Filesystem\SemihostFs\SemihostFs\DEBUG\SemihostFs.dll 0x87E7D240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Disk\DiskIoDxe\DiskIoDxe\DEBUG\DiskIoDxe.dll
0x87E7A240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Disk\PartitionDxe\PartitionDxe\DEBUG\PartitionDxe.dll
0x87E75240
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\FatPkg\EnhancedFatDxe\Fat\DEBUG\Fat.dll 0x87E6C240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Disk\UnicodeCollation\EnglishDxe\EnglishDxe\DEBUG\EnglishDxe.dll
0x87E69240
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Bus\Pci\EhciDxe\EhciDxe\DEBUG\EhciDxe.dll
0x87E61240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Bus\Usb\UsbBusDxe\UsbBusDxe\DEBUG\UsbBusDxe.dll 0x87E58240
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Bus\Usb\UsbMassStorageDxe\UsbMassStorageDxe\DEBUG\UsbMassStorageDxe.dll
0x87E52240
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\Ebl\Ebl\DEBUG\Ebl.dll 0x87160240

The 2nd argument to sym is optional and it defines the image type
represented by the file name string. The image loaded and relocated in
system memory will always be a PE/COFF image as called out in the UEFI
specification, but the symbol files can be in different formats. If the
compiler produces an ELF or Mach-O image the edk2 build system converts
it to a PE/COFF image. The original ELF or Mach-O contain the symbolic
debug information and this is what the debugger needs to load. In both
ELF and Mach-O images the image header is not loaded into system memory,
but it is for PE/COFF. This means the PE/COFF image loaded into system
memory starts with a PE/COFF header. The original ELF or Mach-O file,
that contains the symbols for debug, does not contain a PE/COFF (or any
other) header in the linked image so when we tell the debugger about the
location the ELF or Mach-O image was loaded we need to add in the size
of the PE/COFF header. The default for the sym command is to add in the
size of the PE/COFF header to the image load address, luckily the size
of the PE/COFF header is a field in the PE/COFF header so it is easy to
calculate. If a 2nd argument is present the size of the PE/COFF header
is not added in and as you see below you get the actually load address
of the PE/COFF image. As you can see in these two examples the size of
the PE/COFF header is 0x240.

    BeagleEdk2>sym "add-symbol-file %a 0x%x" PECOFF
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Core\Dxe\DxeMain\DEBUG\DxeCore.dll
0x87F2F000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Core\RuntimeDxe\RuntimeDxe\DEBUG\RuntimeDxe.dll 0x87EDA000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\SecurityStubDxe\SecurityStubDxe\DEBUG\SecurityStubDxe.dll
0x87EB3000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Variable\EmuRuntimeDxe\EmuVariableRuntimeDxe\DEBUG\EmuVariableRuntimeDxe.dll
0x87ED5000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\EmbeddedMonotonicCounter\EmbeddedMonotonicCounter\DEBUG\EmbeddedMonotonicCounter.dll
0x87ED3000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\SimpleTextInOutSerial\SimpleTextInOutSerial\DEBUG\SimpleTextInOutSerial.dll
0x87EB0000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\ResetRuntimeDxe\ResetRuntimeDxe\DEBUG\Reset.dll 0x87EAD000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\RealTimeClockRuntimeDxe\RealTimeClockRuntimeDxe\DEBUG\RealTimeClock.dll
0x87EAB000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\MetronomeDxe\MetronomeDxe\DEBUG\MetronomeDxe.dll 0x87EA9000
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\Flash\Flash\DEBUG\NandFlash.dll 0x87EA6000
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\SmbusDxe\Smbus\DEBUG\Smbus.dll 0x87EA3000
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\Gpio\Gpio\DEBUG\Gpio.dll 0x87EA1000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\InterruptDxe\InterruptDxe\DEBUG\BeagleBoardInterruptDxe.dll
0x87E9E000
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\BeagleBoardPkg\Bds\Bds\DEBUG\BeagleBoardBds.dll
0x87E9A000
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\ArmPkg\Drivers\CpuDxe\CpuDxe\DEBUG\ArmCpuDxe.dll
0x87E92000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\CapsuleRuntimeDxe\CapsuleRuntimeDxe\DEBUG\CapsuleRuntimeDxe.dll
0x87ED0000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\TimerDxe\TimerDxe\DEBUG\BeagleBoardTimerDxe.dll 0x87E8F000
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\TPS65950Dxe\TPS65950\DEBUG\TPS65950.dll
0x87E8C000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\WatchdogTimerDxe\WatchdogTimer\DEBUG\WatchdogTimer.dll
0x87E89000
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\MMCHSDxe\MMCHS\DEBUG\MMCHS.dll 0x87E85000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\Omap35xxPkg\PciEmulation\PciEmulation\DEBUG\BeagleBoardPciEmulation.dll
0x87E81000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\ArmPkg\Filesystem\SemihostFs\SemihostFs\DEBUG\SemihostFs.dll 0x87E7D000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Disk\DiskIoDxe\DiskIoDxe\DEBUG\DiskIoDxe.dll
0x87E7A000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Disk\PartitionDxe\PartitionDxe\DEBUG\PartitionDxe.dll
0x87E75000
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\FatPkg\EnhancedFatDxe\Fat\DEBUG\Fat.dll 0x87E6C000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Universal\Disk\UnicodeCollation\EnglishDxe\EnglishDxe\DEBUG\EnglishDxe.dll
0x87E69000
add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Bus\Pci\EhciDxe\EhciDxe\DEBUG\EhciDxe.dll
0x87E61000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Bus\Usb\UsbBusDxe\UsbBusDxe\DEBUG\UsbBusDxe.dll 0x87E58000
add-symbol-file
c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\MdeModulePkg\Bus\Usb\UsbMassStorageDxe\UsbMassStorageDxe\DEBUG\UsbMassStorageDxe.dll
0x87E52000
    add-symbol-file c:\work\edk2\Build\BeagleBoard\DEBUG_RVCT31\ARM\EmbeddedPkg\Ebl\Ebl\DEBUG\Ebl.dll 0x87160000

Please note the three sym examples are from the same system that was
built using the ARM RVCT 3.1 compiler. The only difference is the
arguments used to the sym command. The 2nd option does not do any
checking for type of the symbol file it just adjusts the load address.

## How do I port a new SoC to the edk2

## How do I write an edk2 driver

## What is a Library Class vs. a Library Instance

## What is a PCD

If you add -y PCD.log -Y PCD to the build command a log file, PCD.log,
will be created relative to the workspace (usually the edk2 directory).
The following is the first section of the PCD.log file:

    ===============================================================================
    Platform Configuration Database Report
    ===============================================================================
      *P  - Platform scoped PCD override in DSC file
      *F  - Platform scoped PCD override in FDF file
      *M  - Module scoped PCD override in DSC file
      *C  - Library has a constructor
      *D  - Library has a destructor
      *CD - Library has both a constructor and a destructor
    ===============================================================================

    ===============================================================================
    PLATFORM: c:\work\edk2\BeagleBoardPkg\BeagleBoardPkg.dsc
    ===============================================================================
    gArmTokenSpaceGuid
        PcdCpuDxeProduceDebugSupport                        :   FLAG  (BOOLEAN) = FALSE
        PcdArmCacheOperationThreshold                       :  FIXED   (UINT32) = 1024
     *P PcdCpuVectorBaseAddress                             :  FIXED   (UINT32) = 0x80000000
                                                                    DEC DEFAULT = 0xfff00000
     *P PcdCpuResetAddress                                  :  FIXED   (UINT32) = 0x80008000
                                                                    DEC DEFAULT = 0x00000000

    gEfiMdePkgTokenSpaceGuid
        PcdVerifyNodeInList                                 :   FLAG  (BOOLEAN) = FALSE
        PcdUgaConsumeSupport                                :   FLAG  (BOOLEAN) = TRUE
        PcdUartDefaultBaudRate                              :  FIXED   (UINT64) = 115200
        PcdMaximumLinkedListLength                          :  FIXED   (UINT32) = 1000000
        PcdMaximumGuidedExtractHandler                      :  FIXED   (UINT32) = 0x10
        PcdMaximumAsciiStringLength                         :  FIXED   (UINT32) = 1000000
     *M                                                             EhciDxe.inf = 0x800fffff
        PcdMaximumUnicodeStringLength                       :  FIXED   (UINT32) = 1000000
        PcdDebugClearMemoryValue                            :  FIXED    (UINT8) = 0xAF
        PcdUefiLibMaxPrintBufferSize                        :  FIXED   (UINT32) = 320
        PcdPerformanceLibraryPropertyMask                   :  FIXED    (UINT8) = 0
        PcdUartDefaultStopBits                              :  FIXED    (UINT8) = 1
        PcdUartDefaultDataBits                              :  FIXED    (UINT8) = 8
        PcdUartDefaultParity                                :  FIXED    (UINT8) = 1
        PcdUefiVariableDefaultPlatformLang                  :  FIXED    (VOID*) = "en-US"
        PcdUefiVariableDefaultLang                          :  FIXED    (VOID*) = "eng"

    gEfiMdeModulePkgTokenSpaceGuid
        PcdFrameworkCompatibilitySupport                    :   FLAG  (BOOLEAN) = FALSE
        PcdSupportUpdateCapsuleReset                        :   FLAG  (BOOLEAN) = FALSE
        PcdVariableCollectStatistics                        :   FLAG  (BOOLEAN) = FALSE
        PcdUnicodeCollationSupport                          :   FLAG  (BOOLEAN) = TRUE
        PcdUnicodeCollation2Support                         :   FLAG  (BOOLEAN) = TRUE
        PcdTurnOffUsbLegacySupport                          :   FLAG  (BOOLEAN) = FALSE
        PcdLoadModuleAtFixAddressEnable                     :  FIXED   (UINT64) = 0
        PcdMaxSizePopulateCapsule                           :  FIXED   (UINT32) = 0x6400000
        PcdMaxSizeNonPopulateCapsule                        :  FIXED   (UINT32) = 0xa00000
        PcdHwErrStorageSize                                 :  FIXED   (UINT32) = 0x0000
        PcdMaxVariableSize                                  :  FIXED   (UINT32) = 0x400
        PcdEmuVariableNvStoreReserved                       :  FIXED   (UINT64) = 0
        PcdMaxHardwareErrorVariableSize                     :  FIXED   (UINT32) = 0x8000
        PcdVariableStoreSize                                :  FIXED   (UINT32) = 0x10000
        PcdLoadFixAddressBootTimeCodePageNumber             :  PATCH   (UINT32) = 0
        PcdLoadFixAddressRuntimeCodePageNumber              :  PATCH   (UINT32) = 0

    gEmbeddedTokenSpaceGuid
     *P PcdCacheEnable                                      :   FLAG  (BOOLEAN) = TRUE
                                                                    DEC DEFAULT = FALSE
     *P PcdPrePiProduceMemoryTypeInformationHob             :   FLAG  (BOOLEAN) = TRUE
                                                                    DEC DEFAULT = FALSE
        PcdEmbeddedDirCmd                                   :   FLAG  (BOOLEAN) = TRUE
     *P PcdEmbeddedMacBoot                                  :   FLAG  (BOOLEAN) = TRUE
                                                                    DEC DEFAULT = FALSE
        PcdEmbeddedIoEnable                                 :   FLAG  (BOOLEAN) = FALSE
        PcdEmbeddedHwDebugCmd                               :   FLAG  (BOOLEAN) = TRUE
        PcdEmbeddedHobCmd                                   :   FLAG  (BOOLEAN) = TRUE
        PcdEmbeddedScriptCmd                                :   FLAG  (BOOLEAN) = FALSE
     *P PcdEmbeddedPciDebugCmd                              :   FLAG  (BOOLEAN) = TRUE
                                                                    DEC DEFAULT = FALSE
        PcdGdbSerial                                        :   FLAG  (BOOLEAN) = FALSE
     *F PcdEmbeddedFdSize                                   :  FIXED   (UINT32) = 0x00080000
                                                                    DEC DEFAULT = 0x0000000
     *F PcdEmbeddedFdBaseAddress                            :  FIXED   (UINT32) = 0x80008000
                                                                    DEC DEFAULT = 0xffff0000
     *F PcdFlashFvMainSize                                  :  FIXED   (UINT32) = 0x0007FDF8
                                                                    DEC DEFAULT = 0x0
        PcdPrePiStackSize                                   :  FIXED   (UINT32) = 0x20000
     *P PcdPrePiStackBase                                   :  FIXED   (UINT32) = 0x87FE0000
                                                                    DEC DEFAULT = 0
     *F PcdFlashFvMainBase                                  :  FIXED   (UINT32) = 0x80008208
                                                                    DEC DEFAULT = 0x0
        PcdPrePiTempMemorySize                              :  FIXED   (UINT32) = 0
        PcdMemoryTypeEfiACPIReclaimMemory                   :  FIXED   (UINT32) = 0
        PcdMemoryTypeEfiACPIMemoryNVS                       :  FIXED   (UINT32) = 0
        PcdMemoryTypeEfiReservedMemoryType                  :  FIXED   (UINT32) = 0
     *P PcdMemoryTypeEfiRuntimeServicesCode                 :  FIXED   (UINT32) = 40
                                                                    DEC DEFAULT = 0
     *P PcdMemoryTypeEfiBootServicesData                    :  FIXED   (UINT32) = 3000
                                                                    DEC DEFAULT = 0
     *P PcdMemoryTypeEfiLoaderCode                          :  FIXED   (UINT32) = 10
                                                                    DEC DEFAULT = 0
        PcdPrePiBfvBaseAddress                              :  FIXED   (UINT32) = 0
        PcdPrePiBfvSize                                     :  FIXED   (UINT32) = 0
     *P PcdMemoryTypeEfiRuntimeServicesData                 :  FIXED   (UINT32) = 80
                                                                    DEC DEFAULT = 0
     *P PcdPrePiHobBase                                     :  FIXED   (UINT32) = 0x80001000
                                                                    DEC DEFAULT = 131072
        PcdMemoryTypeEfiLoaderData                          :  FIXED   (UINT32) = 0
        PcdPrePiCpuMemorySize                               :  FIXED    (UINT8) = 32
     *P PcdMemoryTypeEfiBootServicesCode                    :  FIXED   (UINT32) = 400
                                                                    DEC DEFAULT = 0
        PcdPrePiCpuIoSize                                   :  FIXED    (UINT8) = 0
        PcdEmbeddedPerformanceCounterFreqencyInHz           :  FIXED   (UINT64) = 0x0000000
     *P PcdEmbeddedFdPerformanceCounterPeriodInNanoseconds  :  FIXED   (UINT32) = 77
                                                                    DEC DEFAULT = 0x0000000
        PcdInterruptBaseAddress                             :  FIXED   (UINT32) = 0x38e00000
        PcdTimerPeriod                                      :  FIXED   (UINT32) = 100000
     *P PcdEmbeddedAutomaticBootCommand                     :  FIXED    (VOID*) = ""
                                                                    DEC DEFAULT = L""
     *P PcdEmbeddedPrompt                                   :  FIXED    (VOID*) = "BeagleEdk2"
                                                                    DEC DEFAULT = "Ebl"
        PcdEmbeddedShellCharacterEcho                       :  FIXED  (BOOLEAN) = TRUE
        PcdEmbeddedDefaultTextColor                         :  FIXED   (UINT32) = 0x07
        PcdEmbeddedMemVariableStoreSize                     :  FIXED   (UINT32) = 0x10000
        PcdGdbMaxPacketRetryCount                           :  FIXED   (UINT32) = 10000000

    gOmap35xxTokenSpaceGuid
        PcdBeagleConsoleUart                                :  FIXED   (UINT32) = 3
        PcdBeagleBoardIRAMFullSize                          :  FIXED   (UINT32) = 0x00000000
        PcdBeagleFreeTimer                                  :  FIXED   (UINT32) = 4
        PcdBeagleGpmcOffset                                 :  FIXED   (UINT32) = 0x00000000
        PcdBeagleMMCHS1Base                                 :  FIXED   (UINT32) = 0x00000000
        PcdBeagleArchTimer                                  :  FIXED   (UINT32) = 3

    ===============================================================================
    ===============================================================================
    ...

## What is UEFI PI

The UEFI PI specification utilizes some EFI concepts to define how to
build modular chunks of code in various phases of the firmwares boot
flow. Please see the [UEFI and PI Wiki](../../reference/external-resources/uefi_and_pi_wiki.md) for more information.
