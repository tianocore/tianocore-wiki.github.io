# UDK2010.SR1.Up1 How To Build

## Build Instructions

Download the UDK2010.SR1.UP1.P1 Release with expanded workspace
directories
[Download](https://sourceforge.net/projects/edk2/files/UDK2010%20Releases/UDK2010.SR1.UP1.P1/UDK2010.SR1.UP1.P1.Complete.MyWorkSpace.zip/download)

## UDK2010.SR1.UP1.P1 Release Files / Directories

What is included in the Downloaded zip file

- UDK2010.SR1.UP1.P1.MyWorkSpace.zip
- BaseTools(Windows).zip
- BaseTools(Unix).tar
- Documents

## Windows System Configuration

**Microsoft Windows 7 Ultimate 64-bit\***

1\. Setup Build Environment

- 1\) Install Microsoft Visual Studio 2008\* SP1 in the build machine
  and make sure that AMD64 complier was selected when installing.

2\. Extract Common Source Code

- 1\) Extract files in \[UDK2010.SR1.MyWorkSpace.zip\] to the working
  space directory (e.g C:). Note the Directory "MyWorkSpace" will be
  created as a result. In this case, it is C:\MyWorkspace.
- 2\) There are two BaseTools package one is for Windows system and
  another is for UNIX-Like system. Please make sure
  BaseTools(Windows)\_UDK2010.SR1.zip is used here. Expand the
  appropriate BaseTools to C:\MyWorkSpace

3\. Generate OpenSSL\* Crypto Library Note: this does not need to be
done for Nt32

- Open file
  "C:\MyWorkspace\CryptoPkg\Library\OpensslLib\Patch-HOWTO.txt" and
  follow the instruction to install OpenSSL\* for UEFI building.

4\. Build Steps \*\*\* NT32 \*\*\*

- 1\) Open a command prompt, type command "cd C:\MyWorkspace" to enter
  the workspace directory, and then type command

\> edksetup --nt32

to initialize the working environment. See also:
[Windows systems ToolChain Matrix](../../../build-tooling/environment-setup/windows_systems_toolchain_matrix.md)
for how to change the TOOL_CHAIN_TAG for supported compiler
combinations.

- 2\) Type below commands to build platforms (below assumes Microsoft
  Visual Studio 2008)

\> build -t VS2008x86

Note: There are two methods to select the tool chain (Use Microsoft
Visual Studio 2008\* as sample):

- 1\. Update TOOL_CHAIN_TAG in file Conf/target.txt: TOOL_CHAIN_TAG =
  VS2008
- 2\. Add -t build option in command line: "build -t VS2008 ... "

For 32-bit VS2008 on 64-bit WINDOWS OS, VS2008x86 should be selected
instead of VS2008. Please refer to tools_def.txt for all supported tool
chains and detailed descriptions. (tools_def.txt will be generated at
Conf directory after running "edksetup".)

- 3\. Note Microsoft Visual Studio\* 2010 is supported with -t VS2010 or
  -t VS2010x86

## Unix-Like System Configuration

### Ubuntu

DistributorID: Ubuntu\*

Description: Ubuntu 10\*

Release: Ubuntu 10.10\*

Codename: Karmic\*

1\. Extract Common Source Code

- 1\) Create a working space directory in the build machine, for
  example, ~/src/
- 2\) Extract files in \[UDK2010.SR1.MyWorkSpace.zip\] the working space
  directory. In this case, it is ~/src/MyWorkSpace where /MyWorkSpace is
  created.
- 3\) There are two BaseTools packages, one is for Windows system and
  another is for UNIX-Like system. Please make sure
  BaseTools(Unix)\_UDK2010.SR1.tar is used here.

2\. Generate OpenSSL\* Crypto Library Note: This does not need to be
done for Nt32 Open file
"~/src/MyWorkspace/CryptoPkg/Library/OpensslLib/Patch-HOWTO.txt" and
follow the instruction to install OpenSSL\* for UEFI building.

3\. See How to Set up Build environment
[Using EDK II with Native GCC](../../../build-tooling/environment-setup/using_edk_ii_with_native_gcc.md)
for newer versions of Linux

- a\) Please note that here the root is "~/src/MyWorkSpace" instead of
  "~/src/edk2"
- b\) Make sure BaseTools is built and required software like iASL
  compiler is installed well.
- c\) Some operations need switch to user "root" to execute.

4\. Build Steps \*\*\* Nt32 \*\*\*

- 1\) Open a terminal and type "cd ~/src/MyWorkSpace" to enter the
  workspace directory.
- 2\) Initialize the build environment by typing

\>. edksetup.sh BaseTools

- 3\) Type below commands to build platforms

\> build -t GCC44
