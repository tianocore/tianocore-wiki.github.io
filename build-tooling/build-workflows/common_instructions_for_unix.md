# Common Instructions For Unix

## Common instructions for Unix

`Note: New build instructions are available. It is recommended to start with the new instructions if learning how to`
`build edk2 for the first time. This page is retained for reference.`

New instructions: [Build Instructions](build_instructions.md)

A significant portion of the steps are common on the various UNIX-like
platforms. You should start with the instructions for the operating
system that most closely matches your platform, and it will direct you
here at the appropriate time.

Note these instructions are not for current Linux distributions, only
UNIX-like systems that do not work with the
[Using EDK II with Native GCC](../environment-setup/using_edk_ii_with_native_gcc.md)
instructions. Please follow the
[Using EDK II with Native GCC](../environment-setup/using_edk_ii_with_native_gcc.md)
guide for mainstream Linux distros.

### Get the edk2 source tree using Git

    bash$ mkdir ~/src
    bash$ cd ~/src
    bash$ git clone https://github.com/tianocore/edk2

#### For EDKII project developers

- Clone the EDK II project repository
  - git clone [https://github.com/tianocore/edk2](https://github.com/tianocore/edk2)
- Change to the edk2 directory
- Build the tools
  - make -C BaseTools
- Run the edksetup.sh script
  - . edksetup.sh

When the above steps are done, you can work in the edk2 directory for
code development.

### Build the EDK II BaseTools

    bash$ make -C edk2/BaseTools

### Build gcc x64 UEFI cross compiler

In order to build UEFI images for x64, you will need to build a
cross-compiler build of gcc. This can take quite a while to complete,
possibly several hours on older systems. But, a Python script has been
provided to automate this build process.

Note: This is only needed if behind a internet firewall!

    bash$ export http_proxy=https://proxy.domain.com:proxy_port

To build gcc for x64, use these commands (this will take quite a while
to complete):

    bash$ cd ~/src/edk2/BaseTools/gcc
    bash$ ./mingw-gcc-build.py --arch=x64 \
      --prefix=~/programs/gcc/x64

### Setup build shell environment

    bash$ cd ~/src/edk2
    bash$ export EDK_TOOLS_PATH=~/src/edk2/BaseTools
    bash$ . edksetup.sh BaseTools

### Modify Conf Files

You will need to edit the Conf/tools_def.txt and Conf/target.txt files.
These changes will enable the MdeModulePkg to be built using the gcc x64
compiler.

#### Enable GCC X64 Cross-Compiler

For the Conf/tools_def.txt file, find the following entry and comment
the line out:

    DEFINE UNIXGCC_X64_PETOOLS_PREFIX  = /opt/tiano/x86_64-pc-mingw64/x86_64-pc-mingw64/bin/

Next, find the following entry and uncomment the line:

    DEFINE UNIXGCC_X64_PETOOLS_PREFIX  = ENV(HOME)/programs/gcc/x64/bin/x86_64-pc-mingw32-

#### Set Build Target Information

For the Conf/target.txt file, find the following lines:

    ACTIVE_PLATFORM       = Nt32Pkg/Nt32Pkg.dsc
    TARGET_ARCH           = IA32
    TOOL_CHAIN_TAG        = MYTOOLS

And change the cooresponding lines to match these:

    ACTIVE_PLATFORM       = MdeModulePkg/MdeModulePkg.dsc
    TARGET_ARCH           = X64
    TOOL_CHAIN_TAG        = UNIXGCC

### Build Hello World! (and the rest of MdeModulePkg)

Now you should be able to simply run the build command to compile the
MdeModulePkg.

    bash$ build

As a tangible result of the build, you should have the HelloWorld UEFI
X64 application. If you have a X64 UEFI system available to you, then
this application should be able to run successfully under the shell.

    bash$ ls Build/MdeModule/DEBUG_UNIXGCC/X64/HelloWorld.efi

## Enabling Other Tools

The above showed how to setup an X64 build environment for building the
core MdeModulePkg. However, other packages may require additional tools
such as an IA32 cross-compiler and an ASL compiler. The steps to build
these tools are described in this section.

### Build gcc IA32 UEFI cross compiler

In order to build UEFI images for IA32, you will need to build a
cross-compiler build of gcc. This can take quite a while to complete,
possibly several hours on older systems. But, a Python script has been
provided to automate this build process.

Note: This is only needed if behind a internet firewall!

    bash$ export http_proxy=https://proxy.domain.com:proxy_port

To build gcc for IA32, use these commands (this will take quite a while
to complete):

    bash$ cd ~/src/edk2/BaseTools/gcc
    bash$ ./mingw-gcc-build.py --arch=ia32 \
      --prefix=~/programs/gcc/ia32

#### Modify Conf Files

Once the cross-compiler has been successfully built the
Conf/tools_def.txt will need to be updated so the cross-compiler can be
used.

Find the following statement in Conf/tools_def.txt and comment the line
out:

    DEFINE UNIXGCC_IA32_PETOOLS_PREFIX = /opt/tiano/i386-tiano-pe/i386-tiano-pe/bin/

Next, find the following statement and uncomment the line:

    DEFINE UNIXGCC_IA32_PETOOLS_PREFIX = ENV(HOME)/programs/gcc/ia32/bin/i686-pc-mingw32-

To enable building your target image with IA32 support the
Conf/target.txt will also need to be modified.

Find the TARGET_ARCH definition in Conf/target.txt and change the
corresponding line to match this

    TARGET_ARCH           = IA32

### Build the Intel ASL (iasl) compiler

The Intel ASL compiler is not required for all edk2 developers. It is
unlikely that UEFI Application or UEFI Driver builds will need an ASL
compiler. But, if you are building an entire system firmware image, then
you may need an ASL compiler. For example, the edk2 OVMF sample platform
does require an ASL compiler in order to be built.

First, download the latest ACPI-CA release from [https://www.acpica.org](https://www.acpica.org).

**OS X users**: At this time, the latest versions of ACPI-CA are not
building on Mac OS X, so please use the release from 20081031 instead.

    bash$ cd ~/src
    bash$ wget https://www.acpica.org/download/acpica-unix-20090521.tar.gz
    bash$ tar -zxf acpica-unix-20090521.tar.gz
    bash$ make -C acpica-unix-20090521/compiler
    bash$ ln -s ~/src/acpica-unix-20090521/compiler/iasl ~/programs/iasl

#### Modify Conf Files

Once the Intel ASL compiler has been successfully built the
Conf/tools_def.txt will need to be updated so the ASL compiler can be
used.

Find the following statement in Conf/tools_def.txt and comment the line
out:

    DEFINE UNIX_IASL_BIN           = /usr/bin/iasl

Next, find the following statement and uncomment the line:

    DEFINE UNIX_IASL_BIN           = $(HOME)/programs/iasl

### Build [OVMF](https://www.tianocore.org/ovmf/)

Once your build environment is set up you might be interested in
building the [OVMF](https://www.tianocore.org/ovmf/) platform which is
included in the main edk2 source tree. Since
[OVMF](https://www.tianocore.org/ovmf/) builds a full system firmware
image this may be of interest to UEFI system firmware developers.
