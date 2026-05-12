# Beagleboardpage

## Beagle Board

To find the latest instructions to build the BeagleBoard on Linux, see
[BeagleBoardPkg](beagle_board_pkg.md).

## Get the edk2 source tree and build BeagleBoardPkg in Windows® Cygwin Bash shell

The following instructions assume you have the ARM RealView Development
Suite v3.1 and Cygwin installed on Windows. Other versions of the
RealView Development Suite should work, but only v3.1 and v4.0 have been
tested.

If you use the command line version of subversion, then you can easily
checkout the edk2 and source to the FAT32 driver to the /cygdrive/c/edk2
directory with the following commands:

    /cygdrive/c$ svn co https://sourceforge.net/p/edk2/code/HEAD/tree/trunk/edk2 edk2 --username guest
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
Board_EFI​ \_flashboot.fd.
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
    /cygdrive/c/edk2/BeagleBoardPkg$ -y report.log -v

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
    OMAP3 beagleboard.org # fatload mmc 0 80200000 BeagleBoard_EFI_flashboot.fd
    OMAP3 beagleboard.org # nandecc hw
    OMAP3 beagleboard.org # nand erase 0 80000
    OMAP3 beagleboard.org # nand write 80200000 0 80000

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

If we modify the target address of the EFI image it is possible to boot
it on top of U-Boot. This is a useful way to do development so you can
test your EFI image before you place it in NAND.

Step \#1
We need to modify target address of the EFI image to boot it on top of
u-boot.
In edk2/BeagleBoardPkg/​BeagleBoardPkg.fdf change:

    ####BaseAddress = 0x80208000|gEmbedded​TokenSpaceGuid.PcdEm​beddedFdBaseAddress #The base address of the FLASH Device.
    BaseAddress = 0x80008000|gEmbedded​TokenSpaceGuid.PcdEm​beddedFdBaseAddress #The base address of the FLASH Device.

To:

    BaseAddress = 0x80208000|gEmbedded​TokenSpaceGuid.PcdEm​beddedFdBaseAddress #The base address of the FLASH Device.
    ####BaseAddress = 0x80008000|gEmbedded​TokenSpaceGuid.PcdEm​beddedFdBaseAddress #The base address of the FLASH Device.

Step \#2
Then rebuild:

    /cygdrive/c/edk2$ cd /cygdrive/c/edk2/BeagleBoardPkg
    /cygdrive/c/edk2$ ./build.sh

Step \#3
Copy
edk2\Build\BeagleB​oard\DEBUG_RVCT31CY​GWIN\FV\BeagleBoar​d_EFI_flashboot.fd
to a MMC/SD card. Note: this path includes the name of the compiler, so
if you use a different one, it may be slightly different.
Place SD card back in Beagle Board
power cycle while holding down USER button
hit a key on the serial console to stop u-boot from loading Linux

Step \#4
At the prompt (currently OMAP3 beagleboard.org \# ) type these following
u-boot commands to boot EFI code from DRAM:

    OMAP3 beagleboard.org # mmcinit
    OMAP3 beagleboard.org # fatload mmc 0 80200000 BeagleBoard_EFI_flashboot.fd
    OMAP3 beagleboard.org # go 80208208'

You should see DEBUG prints from EFI, and when you get to the prompt it
will look like this:

    Embedded Boot Loader (EBL) prototype. Built at 16:18:20 on Dec 9 2009
    THE PROGRAM IS DISTRIBUTED UNDER THE BSD LICENSE ON AN 'AS IS' BASIS,
    WITHOUT WARRANTIES OR REPRESENTATIONS OF ANY KIND, EITHER EXPRESS OR IMPLIED.
    Please send feedback to dev@edk2.tianocore.org
    BeagleEdk2>

If your Beagle Board has u-boot in the NAND you don't need to make the
SD card bootable via u-boot. You can just copy Beagle
Board_EFI_flashboot.fd to the SD card.
It should also be possible to boot EFI from USB using the following
U-Boot commands:

    OMAP3 beagleboard.org # usb reset
    OMAP3 beagleboard.org # usb scan
    OMAP3 beagleboard.org # fatload USB 0:1 80200000 BeagleBoard_EFI_flashboot.fd
    OMAP3 beagleboard.org # go 80208208

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

## Get the edk2 source tree and build BeagleBoardPkg in Windows® DOS Box

The following instructions assume you have the ARM RealView Development
Suite v3.1 and Subversion installed on Windows. Other versions of the
RealView Development Suite should work, but only v3.1 and v4.0 have been
tested.

If you don't have Cygwin installed you can't use the build.sh Bash
script in the BeagleBoardPkg directory. Building on Windows is a little
simpler as binary versions of all the tools are checked in, but if you
don't run the build.sh script the RealView Debugger scripts will not be
created.

If you use the command line version of subversion, then you can easily
checkout the edk2 to the C:\edk2 directory with the following command:

    C:\> svn co     https://sourceforge.net/p/edk2/code/HEAD/tree/trunk/edk2 C:\edk2 --username guest
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
