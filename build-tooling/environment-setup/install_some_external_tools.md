# Install Some External Tools

Getting and Installing the Required Tools for Building [EDK
II](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II/).

Note: The **step-by-step** instructions also cover installing
pre-requisite build tools for some build environments.

## Requirements

- An IA32 or X64 based development workstation (IPF workstations are not
  supported)
- Microsoft Windows XP or Vista, Apple Mac OS/X (10.4 or later) or Linux
  operating system
- Disk space for compiler tools
- Minimum of 1GB of disk space for edk2 development tree and output
  files
- Minimum of 512MB (1GB recommended) of system memory

The one of the following is required to be able to obtain the EDK II.

| Name              | URL                                          |
|-------------------|----------------------------------------------|
| TortoiseSVN       | [https://tortoisesvn.net/downloads](https://tortoisesvn.net/downloads)           |
| Command Line Tool | [https://subversion.apache.org/packages.html](https://subversion.apache.org/packages.html) |

## Third Party Tools

3rdParty Tools must include: a C pre-processor, C compiler, static
linker, dynamic linker, and an assembler and assembly linker. For
creating ACPI tables required by platforms, an ACPI assembler is also
required.

The compiler tool chains are not provided as part of EDK II. Before
purchasing a compiler tool chain, make sure to review the End User
License Agreement (EULA) that comes with the software to ensure that
creating EFI firmware and applications (for commercial purposes) for the
target architecture is permitted.

At least one of the following 3rd party compiler tool chain is required:

| Name | Version | URL | Note |
|----|----|----|----|
| Cygwin | 4.1.2 | [https://www.cygwin.com](https://www.cygwin.com) | Instructions for installation of GCC are included in the BaseTools\gcc directory. |
| Microsoft Visual Studio | 2005 Professional | [https://msdn2.microsoft.com/en-us/vstudio](https://msdn2.microsoft.com/en-us/vstudio) | The default tool chain for the IA32 and X64 builds. |
| Microsoft Visual Studio | 2005 Team Suite | [https://msdn2.microsoft.com/en-us/vstudio](https://msdn2.microsoft.com/en-us/vstudio) | Needed for building IPF targets if the DDK is not installed. |
| Microsoft Visual Studio | 2003 .NET | [https://msdn2.microsoft.com/en-us/vstudio](https://msdn2.microsoft.com/en-us/vstudio) | May only be available through a MSDN subscription. If the DDK is not available, MASM 6.15 can be used for the assembler. |
| Intel C++ Compiler for Windows | 9.1 | [https://www.intel.com](https://www.intel.com) | The Intel Compiler requires a Microsoft Visual Studio installation. |
| Intel C Compiler for EFI Byte Code | 1.2 | [https://www.intel.com/cd/software/products/asmo-na/eng/compilers/efibc/index.htm](https://www.intel.com/cd/software/products/asmo-na/eng/compilers/efibc/index.htm) |  |
| Microsoft Windows Driver Development Kit6 (DDK) | 3790.1830 |  | The default tool chain for IPF builds. To download and use the DDK, you must burn the ISO file to a CD or DVD. |
| Windows Server 2003 SP1 DDK |  |  |  |
| Microsoft ACPI Source Language Assembler | 3.0.0NT or later | [https://www.microsoft.com/whdc/system/pnppwr/powermgmt/default.mspx](https://www.microsoft.com/whdc/system/pnppwr/powermgmt/default.mspx) |  |
| Intel ACPI Component Architecture | 20060113 or later \|-[https://www.intel.com/technology/iapc/acpi/downloads.htm](https://www.intel.com/technology/iapc/acpi/downloads.htm) |  |  |

## Windows XP or Windows Vista

After installing the compiler tools and your Subversion client, download
the edk2, read the BuildNotes2.txt file and you will be ready to build
an image.

All builds are started from a command prompt window.

## Cygwin

If you install cygwin (for GCC support under Windows) you should install
it in "Unix" mode. The gcc tool chain will not compile in "DOS" mode.

For Cygwin, you may also find the
[Unix-like systems
step-by-step instructions](unix_like_systems.md) valuable.

## Environment Variables

Environment variables are case sensitive. You must use the exact case as
in the examples of this document. Even though windows does not care
about case, other operating systems that are supported do care about
case.

You need to set WORKSPACE to the location of the edk2 directory that you
pulled from Subversion. For example:

set WORKSPACE=c:\workspace\edk2

It is recommended that you wrap up all the environment variables above
into a script that you can launch each time you begin to do development
in your EDK II workspace.

Multiple Workspaces are also supported see how:
[Multiple_Workspace](../build-workflows/multiple_workspace.md)

## First Build

The **step-by-step** documents how to build the MdeModulePkg under
various environments.
