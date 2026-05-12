# ArmPkg Ds5

## Set up ARM DS-5 for UEFI development

This tutorial uses DS5 v5.12.
Even if this tutorial is based on Linux; it should also work on Windows
(after converting the file paths).

1\. Start DS-5.

**Note:** This tutorial assumes DS-5 has been configured to debug
your target (hardware or RTSM model).
Help can be found here to add your RTSM model to DS-5 if not already
done:
[https://developer.arm.com/documentation](https://developer.arm.com/documentation)

2\. Import the Tianocore project from the Sourceforge repository

**Note:** To install the Eclipse Git plugin in DS-5, see:
[https://www.eclipse.org/egit/](https://www.eclipse.org/egit/)
Before installing the git (or svn) plugin, check your Eclipse version
(Help \> About Eclipse Platform). *Example, Eclipse 3.7 = Indigo*
The list of plugins for this Eclipse Platform version is available at
[https://download.eclipse.org/releases/indigo](https://download.eclipse.org/releases/indigo)

`- File > Import ...`
`- Git > Projects from GIT`

The Sourceforge git repository (mirror of svn) is
<git://tianocore.git.sourceforge.net/gitroot/tianocore/edk2>. The main
branch is the 'master' branch; it is not required to import the other
branches. ![](ArmDs5-tutorial-1.png "ArmDs5-tutorial-1.png")

3\. After the GIT repository has been imported, select 'Use the New
Project wizard' from the window 'Select a wizard to use for importing
projects'. ![](ArmDs5-tutorial-2.png "ArmDs5-tutorial-2.png")

And choose 'Makefile Project with Existing Code' in 'C/C++ project'
![](ArmDs5-tutorial-3.png "ArmDs5-tutorial-3.png")

4\. It is safer to create a development branch to separate your
development from the public repository.
![](ArmDs5-tutorial-4.png "ArmDs5-tutorial-4.png")

5\. Build the UEFI firmware for your platform. Example for the ARM RTSM
Versatile Express Cortex A9x4, defined by the DSC file:
ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-RTSM-A9x4.dsc

`- Project > Properties`

![](ArmDs5-tutorial-5.png)
*ArmDs5-tutorial-5.png*

- Change the default 'make' Build command to use the Tianocore Makefile:

    make -f ArmPlatformPkg/Scripts/Makefile

![](ArmDs5-tutorial-5-2.png)
*ArmDs5-tutorial-5-2.png*

- Add the Environment Variables:

`- EDK2_DSC=ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-RTSM-A9x4.dsc`

![](ArmDs5-tutorial-6.png)
*ArmDs5-tutorial-6.png*

- The supported environment variables are:

`EDK2_ARCH=ARM|IA32|X64|... (default: ARM)`
`EDK2_TOOLCHAIN=RVCTLINUX|ARMLINUXGCC|ARMGCC|... (default: RVCTLINUX)`
`EDK2_BUILD=DEBUG|RELEASE (default: DEBUG)`
`EDK2_MACROS=...`

![](ArmDs5-tutorial-7.png)
*ArmDs5-tutorial-7.png*
**Note:** On Windows, it is required to add the locations of nmake
and the prebuilt binaries of the Tianocore BaseTools to the PATH
environment variable.
You can add a similar PATH to your project environment variables in
addition to the environment variables above:

`PATH=C:\Program Files\Microsoft Visual Studio 9.0\VC\bin; C:\git\edk2\BaseTools\Bin\Win32;${Path}`

6\. Build your project

`- Project > Build All`

![](ArmDs5-tutorial-8.png)
*ArmDs5-tutorial-8.png*
7\. Switch to the DS-5 Perspective

8\. Create a new Debug Configuration
![](ArmDs5-tutorial-9.png "ArmDs5-tutorial-9.png")

Prior to DS-5 v5.11, you need to edit the model configuration file in
the DS-5 Configuration Database:

    sudo vim /usr/local/DS-5/sw/debugger/configdb/Boards/ARM/RTSM_VE_Cortex_A9x4/a9_bare_metal.xml

Add the model parameter to the entry :

-C
motherboard.flashloader0.fname="\[YOUR_WORKSPACE_FULLPATH\]/edk2/Build/ArmVExpress-RTSM-A9x4/DEBUG_RVCTLINUX/FV/RTSM_VE_CORTEX-A9_EFI.fd"

From DS-5 v5.11, these parameters can be passed through the DS-5 User
Interface. ![](ArmDs5-tutorial-10.png "ArmDs5-tutorial-10.png")

**Note:**To get the list of supported parameters:

    /usr/local/DS-5/bin/RTSM_VE_Cortex-A9_MPx4 -l

9\. In the 'Debugger' tab, select 'Connect Only'

**Note:**If your development platform waits for a firmware at a
specific location, you can for instance load the binary into the target
binary from 'Execute debugger commands' using the following script:

    interrupt
    if $pc==0
    restore Build/ArmVExpress-RTSM-A9x4/DEBUG_RVCTLINUX/FV/RTSM_VE_CORTEX-A9_EFI.fd binary 0x8000000
    end

This development method might be useful during firmware bring up when
flashing the new firmware takes a significant amount of time.

Example of using DS-5 with the ARM Fast Model:
![](ArmDs5-tutorial-11.png "ArmDs5-tutorial-11.png")

**Note:** On Windows to have a better support of the UEFI Serial
output when using the Fast Model, it is recommended to use Putty instead
of the default telnet terminal.
Putty can be started at any time after the ARM Fast Model has been
started. The default Fast Model telnet connection is localhost:5000.

10\. To debug UEFI you can use the UEFI Python scripts for DS-5
(included in EDK2 repository):

- Example for loading all the symbols at the current 'PC' (Program
  Counter Register) position with DRAM from '0x80000000' to
  '0x80000000+0x40000000'

`- source edk2/ArmPlatformPkg/Scripts/Ds5/cmd_load_symbols.py -m (0x80000000,0x40000000)`

- Example for loading all the symbols loaded in DRAM from '0x80000000'
  to '0x80000000+0x4000000'

`- source edk2/ArmPlatformPkg/Scripts/Ds5/cmd_load_symbols.py -m (0x80000000,0x40000000) -a`

- Example for loading all the symbols at the current 'PC' (Program
  Counter Register) position with a Firmware Volume from '0x08000000' to
  '0x08000000+0x00280000'. Mainly useful during the XIP phase (SEC + PEI
  before relocation to DRAM)

`- source edk2/ArmPlatformPkg/Scripts/Ds5/cmd_load_symbols.py -f (0x08000000,0x00280000)`

- Example for loading all the symbols for the platform with a Firmware
  Volume at 0x08000000 and DRAM at 0x80000000:

`- source edk2/ArmPlatformPkg/Scripts/Ds5/cmd_load_symbols.py -f (0x08000000,0x00280000) -m (0x80000000,0x40000000) -a`

**Note:** If you build UEFI with the build report file 'report.log'
in your EDK2 root directory (add '-y report.log' to your build command
to generate this file) then the script will parse the report.log to get
the address of the Firmware Volume and System Memory for your current
platform. This convenient solution is not recommended if you work with
multiple platforms. The current EDK2 BaseTools (23/10/2012) do not
handle well changing the build options without cleaning the build.

`- source edk2/ArmPlatformPkg/Scripts/Ds5/cmd_load_symbols.py`

To have the list of all supported arguments:

`- source edk2/ArmPlatformPkg/Scripts/Ds5/cmd_load_symbols.py -h`

If you are not familiar with the platform, the way to get arguments for
the script cmd_load_symbols.py is to get those information from the DSC
and FDF EDK2 files of your platform.
Example: -f (0x08000000, 0x00280000) comes from the base address
and size of the FD file in
ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-RTSM-A9x4.fdf

    [FD.RTSM_VE_Cortex-A9_EFI]
    BaseAddress   = 0x08000000|gArmTokenSpaceGuid.PcdFdBaseAddress  # The base address of the Firmware in NOR Flash.
    Size          = 0x00280000|gArmTokenSpaceGuid.PcdFdSize         # The size in bytes of the FLASH Device
    ErasePolarity = 1

-m (0x80000000, 0x40000000) is the base address and size of the DRAM.
This information is also available in
ArmPlatformPkg/ArmVExpressPkg/ArmVExpress-RTSM-A9x4.dsc:

      # System Memory (1GB)
      gArmTokenSpaceGuid.PcdSystemMemoryBase|0x80000000
      gArmTokenSpaceGuid.PcdSystemMemorySize|0x40000000

**Note 1:** DS-5 does not currently recognise '\*.iii' files as ARM
assembly files. It is the reason why when stopping the DS-5 debugger and
calling the script 'cmd_load_symbols.py' a single function may appear in
the callstack.
A workaround is to add the '\*.iii' extension to the ARM assembly files:

    Window > Preferences, General > Editors > File Associations

With this extension association the full callstack should be displayed.

**Note 2:** It is expected to see the warning messages after
executing the command 'cmd_load_symbols.py':

Warning: not possible to load symbols from
/home/olimar01/edk2/Build/Fat/RELEASE_RVCTLINUX/ARM/FatPkg/EnhancedFatDxe/Fat/DEBUG/Fat.dll
Warning: not possible to load symbols from
/home/olimar01/edk2/Build/Fat/RELEASE_RVCTLINUX/ARM/ShellPkg/Shell/DEBUG/Shell.dll

Because the Fat and Shell binaries come as pre-built binaries.
If you want to rebuild these binaries for your platform:
[ArmPkg-Binaries](../../platforms-packages/core-packages/armpkg_binaries.md)

**Note 3:** DS-5 Debugger’s editor does not recognize the '\*.iii' as
assembly files. The workaround is to add a file association for "\*.iii"
to "ARM Assembler Editor" in DS-5 Eclipse: Window \> Preferences,
General \> Editors \> File Associations
