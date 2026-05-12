# How to Build [UDK2018](../../../releases-history/archives/udk2018.md)

```admonish note "Updated Instructions Available"
New build instructions are available. It is recommended to start with the new instructions if learning how to
build edk2 for the first time and not specifically targeting UDK2018. This page is retained for reference.
```

New instructions: [Build Instructions](../../../build-tooling/build-workflows/build_instructions.md)

Table of Contents

1. [HOW TO BUILD (WINDOWS SYSTEM)](#how-to-build-windows-system)
2. [HOW TO BUILD (LINUX-LIKE SYSTEM)](#how-to-build-linux-like-system)

## HOW TO BUILD (WINDOWS SYSTEM)

The steps below are verified on Microsoft Windows 10 Enterprise*:

1. Setup Build Environment
   1. Install Microsoft Visual Studio 2015* in the build machine and make
      sure that AMD64 complier was selected when installing.
   2. Download NASM 2.12.01 or later from [https://www.nasm.us/](https://www.nasm.us/) and install it to
      C:\Nasm. Make sure C:\Nasm is added to system environment variable 'PATH'
   3. Set up for using GitHub for Windows. See:
      [../../../development/tutorials-howto/getting_started_with_edk_ii.md#github-help](../../../development/tutorials-howto/getting_started_with_edk_ii.md#github-help)
   4. Download and install Python2.7.x  [https://www.python.org/](https://www.python.org/) for building the BaseTools
      Default install directory is: C:/Python27
   5. Download the pre-compiled Openssl binary from
      [https://wiki.openssl.org/index.php/Binaries](https://wiki.openssl.org/index.php/Binaries). Search for a
      Windows binary in the list of "_Third Party OpenSSL Related Binary Distributions_" table that will be
      Windows binary. Go to the third party site to download the latest version of Windows x64 86 or Win32 binary.
      Download and extract to C:\Openssl. Make sure C:\Openssl is added to system environment
      variable 'PATH'.

2. Create the full Source Code directory for the UDK2018 release
    1) Create a working space directory in the build machine, for example, C:\MyWorkspace
    2) Download the official UDK2018 release .zip file from the [UDK2018 Release Page](https://github.com/tianocore/edk2/releases/tag/vUDK2018)

    1. Download - UDK2018 edk-vUDK2018 Workspace [Source code (zip file)](https://github.com/tianocore/edk2/archive/vUDK2018.zip)
    2. Extract files in `edk2-vUDK2018` to the working space directory C:\MyWorkspace.
    3. **OR**  Checkout the vUDK2018 Tag from GitHub with the following "git" command
        1. run  `git clone  https://github.com/tianocore/edk2.git vUDK2018`
        2. Go to the vUDK2018 directory, and from there run: `git checkout tags/vUDK2018 -b vUDK2018`
        3. Move all files and folders under "vUDK2018" to "C:\MyWorkspace"

3. Generate OpenSSL* Crypto Library
   1. Open file "C:\MyWorkspace\CryptoPkg\Library\OpensslLib\OpenSSL-HOWTO.txt"
      and follow the instruction to install OpenSSL* for UEFI building.
      For this release, please use OpenSSL-1.1.0g. Download it from
      [https://github.com/openssl/openssl/archive/OpenSSL_1_1_0g.zip](https://github.com/openssl/openssl/archive/OpenSSL_1_1_0g.zip)
      Extract it to C:\MyWorkspace\CryptoPkg\Library\OpensslLib,
      and rename its directory name to openssl

4. Compile the BaseTools See:
   [../../../build-tooling/environment-setup/windows_systems.md#compile-tools](../../../build-tooling/environment-setup/windows_systems.md#compile-tools)
   1. Open a Microsoft Visual Studio* command prompt, type `cd C:\MyWorkspace`
      to enter the  workspace directory
   2. Compile the BaseTools C source tools

    ```batch
    set PYTHON_HOME=C:\Python27
    edksetup.bat Rebuild
    ```

5. Build Steps
   _**NT32**_

   1. Open a Microsoft Visual Studio* command prompt, type `cd C:\MyWorkspace`
      to enter the workspace directory
   2. Use edksetup.bat command to initialize the working environment.
      `edksetup --nt32`
   3. Type following command to build Nt32 platform
      `build -t VS2015x86`
   4. Upon the build completing successfully there should be the UEFI Application "`HelloWorld.efi`" in the
      C:\MyWorkspace\Build\NT32IA32\DEBUG_VS2015x86\IA32 directory

---

## HOW TO BUILD (LINUX-LIKE SYSTEM)

The below steps are verified on Ubuntu 16.04 LTS Desktop*:

1. Setup Build Environment
   1. Follow instructions for setting up the build environment on tianocore.org.
      "[Using EDK II with Native GCC](../../../build-tooling/environment-setup/using_edk_ii_with_native_gcc.md)"

      1. Please notice that here the root is `"~/src/MyWorkspace`" instead
         of `"~/src/edk2"`

      2. Make sure BaseTools is built and required software like iASL compiler
         is installed well.
         1. Some operations need switch to user "root" to execute.
         2. At Ubuntu, you can type `"sudo apt-get install git"` under terminal "Cnt-Alt-T" to
            install git.
   2. Install NASM 2.12.01 or later.
      At Ubuntu, you can type "sudo apt-get install nasm" under terminal to
      install it.
      You can also download the source package from [https://www.nasm.us/](https://www.nasm.us/) and
      install it following the instruction on the website.
   3. Install IASL 20150818 or later.
      At Ubuntu, you can type `"sudo apt-get install iasl"` under terminal to
      install it.
      You can also download the source package from [https://acpica.org/downloads](https://acpica.org/downloads)
      and install it following the instruction on the website.

2. Create the full Source Code directory for the UDK2018 release
   1. Create a working space directory in the build machine, for example, ~/src/MyWorkspace
   2. Download the official UDK2018 release .tar file from the [UDK2018 Release
      Page](https://github.com/tianocore/edk2/releases/tag/vUDK2018)
      1. Download - UDK2018 edk-vUDK2018 Workspace [Source code (tar.gz
         file)](https://github.com/tianocore/edk2/archive/vUDK2018.tar.gz)
      2. Extract files in `edk2-vUDK2018` to the working space directory ~/src/MyWorkspace.
   3. **OR**  Checkout the vUDK2018 tag from GitHub with the following "git" command
      1. Run `"git clone https://github.com/tianocore/edk2.git vUDK2018"`
      2. Go to the vUDK2018 directory, and from there run: `git checkout tags/vUDK2018 -b vUDK2018`
      3. Move all files and folders under "vUDK2018" to "~/src/MyWorkspace"

3. Generate OpenSSL* Crypto Library
   1. Open file "~/src/MyWorkspace/CryptoPkg/Library/OpensslLib/OpenSSL-HOWTO.txt"
      and follow the instruction to install OpenSSL* for UEFI building.
      For this release, please use OpenSSL-1.1.0g.       Download it from
      [https://github.com/openssl/openssl/archive/OpenSSL_1_1_0g.zip](https://github.com/openssl/openssl/archive/OpenSSL_1_1_0g.zip)
      Extract it to ~/src/MyWorkspace/CryptoPkg/Library/OpensslLib,
      and rename its directory name to openssl

4. Build Steps
   _**MdeModulePkg**_

   1. Open a terminal and type `"cd ~/src/MyWorkspace"` to enter the workspace
      directory.
   2. First build the BaseTools by typing
      `"make -C BaseTools"`
   3. Initialize the build environment by typing
      `". edksetup.sh"`.
   4. Type following command to build platforms using GCC v5.4.0
      `"build  -p MdeModulePkg/MdeModulePkg.dsc -t GCC5"`
   5. Upon the build completing successfully there should be the UEFI Application "`HelloWorld.efi`" in the
      ~/src/MyWorkspace/Build/MdeModule/DEBUG_GCC5/IA32 directory

---

If you have questions please email the [edk2-devel](mailto:edk2-devel@lists.01.org) mail list.

See also [Getting Started with EDK II](../../../development/tutorials-howto/getting_started_with_edk_ii.md)
