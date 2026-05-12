# EDK II Build Tools Project

This project is for development of the
[EDK II](../../reference/external-resources/edk_ii.md) Build Tools.
This is the primary set of tools for processing [EDK
II](http://www.tianocore.org/edk2/) content. It contains configuration
templates and source files. The tools support a Makefile based EDK II
build with no additional packages required--the compiler tool chain, an
assembler and optional ACPI assembler are the only additional tools need
to build the EDK II project.

Source code in this project is divided into two types:

- Tools written in C ([ANSI C](https://www.open-std.org/jtc1/sc22/wg14/))
  are primarily for tools that modify binary data structures
- Tools based on Python ([Python](https://www.python.org)) are primarily
  for tools that parse or process text files

Tools must adhere to the following requirements:

- Tools must be able to execute on a wide variety of operating systems.
- Tools written in Python use
  [Python
  Tools](../../platforms-packages/tooling-utilities/python_tools.md) and get converted to Win32 executable binary files before
  they are added to the BaseTools directory in the EDK II project.

To assist developers working with the Python tools, Python has been
provided, along with the Python tools for creating graphical user
interfaces (wxPython) and tools for creating native executable files
(cxFreeze and py2app) for Microsoft\*, Linux\*, and Mac OS/X\*. Python
and the supporting Python package are available via SVN (See Resources
below).

Tested and Released binaries for Microsoft Windows\* 32-bit operating
system are checked into the EDK II project along with the configuration
files. The BaseTools use an INI format for build Meta Data files. You do
not need to down load this project to build EDK II with the BaseTools.
Refer to the BuildNotes2.txt file for details on using the BaseTools for
the build.

Download and setup guide:
[BuildTool Setup Guide](../environment-setup/buildtool_setup_guide.md)

- Prebuilt Windows tools are available at
  [https://github.com/tianocore/edk2-BaseTools-win32.git](https://github.com/tianocore/edk2-BaseTools-win32.git)
  - Note: the Prebuilt Windows tools (Win32 binaries) are only valid for
    the tip of [https://github.com/tianocore/edk2](https://github.com/tianocore/edk2). It is recommended to
    build the source tools for other EDK II branches and projects:
    Please see: [Windows-systems#compile-tools](../environment-setup/windows_systems.md#compile-tools)
- BaseTools content (part of the edk2 project) is available at:
  [https://github.com/tianocore/edk2.git](https://github.com/tianocore/edk2.git)

Project Info: [Project Info
ReadMe](https://github.com/tianocore/edk2/tree/master/BaseTools/ReadMe.txt)

Documentation [User
Manuals](https://github.com/tianocore/edk2/tree/master/BaseTools/UserManuals)

View the [EDK II Tools List](edk_ii_tools_list.md) for a description of
each tool.

## Releases

[UDK2017](../../releases-history/archives/udk2017.md):

- Changes for UDK2017 [BaseTools
  Notes](https://github.com/tianocore-docs/Docs/blob/master/UDK/UDK2017/BaseToolsNotes.md)
- Please see
  [UDK2017](../../releases-history/archives/udk2017.md) wiki
  page for more information on the latest release of Build Tools

UDK2015 :

- [BaseTools.Windows.zip](https://github.com/tianocore/udk/releases/download/UDK2015/BaseTools.Windows.zip)
- [BaseTools.Unix.tar](https://github.com/tianocore/udk/releases/download/UDK2015/BaseTools.Unix.tar)

---

Below Table are Releases for Archive purposes only

### BuildTools Releases

#### Release: June 26, 2012

**Downloads:**

- [TAR](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/BaseTools(Unix)_UDK2010.SR1.UP1.tar/download)
- [.ZIP](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/BaseTools(Windows)_UDK2010.SR1.UP1.zip/download)

**Description:** Primary set of tools for processing EDK II content.

**Tools included:**

- BootSectImage
- Build Utility
- EfiLdrImage
- EfiRom
- Fpd2Dsc
- GenBootSector
- GenCrc32
- GenDepex
- GenFds
- GenFfs
- GenFv
- GenFw
- GenPage
- GenPatchPcdTable
- GenSec
- GenVtf
- UEFI Packaging Tool
- LzmaCompress
- Msa2Inf
- PatchPcdValue
- Spd2Dec
- SplitFile
- TargetTool
- TianoCompress
- Trim
- VfrCompiler
- VolInfo

**Documents:**

*Readme:*

- [ReadMe.txt (baseTools)](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/ReadMe.txt/download)
- [ReadMe.txt (Base Tools–gcc)](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/ReadMe(Gcc).txt/download)
- [ReadMe.txt (Base-Tools-config)](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/ReadMe(Coonf).txt/download)

*General documentation:*

- [UDK2010.SR1.UP1 User Manuals Zip](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/UDK2010.SR1.UP1.UserManuals.zip/download)

*Specifications:* (None listed)

*Technical information:* (None listed)

---

#### Release: Jan 2, 2012

**Downloads:**

- [.TAR](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/BaseTools(Unix)_UDK2010.SR1.tar/download)
- [.ZIP](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/BaseTools(Windows)_UDK2010.SR1.zip/download)

**Description:** Primary set of tools for processing EDK II content.

**Documents:**

*Readme:*

- [ReadMe.txt (baseTools)](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/ReadMe.txt/download)
- [ReadMe.txt (Base Tools–gcc)](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/ReadMe(Gcc).txt/download)
- [ReadMe.txt (Base-Tools-config)](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/ReadMe(Coonf).txt/download)

*General documentation:*

- [User Manuals Zip](https://sourceforge.net/projects/edk2-buildtools/files/BuildTools_Source_Packages/UserManuals.zip/download)

*Specifications:* (None listed)

*Technical information:* (None listed)

## Notes

These tools are under constant development to ensure
[UEFI](../../reference/specs-standards/uefi.md)/PI specification
conformance and to reduce build times.

Your Feedback is critical to making EDK II a success. Please submit any
enhancements, defects, or requests through the
[edk2-devel mailing list](mailto:edk2-devel@lists.01.org?subject=BuildTools&body=BuildTools).

Goto
[edk2-devel](../../community/communications/mailing_lists.md)
to Join the Mailing list

License information: [BSD Plus Patent License](../../reference/legal-licenses/bsd_plus_patent_license.md)

Project owner(s): See:
[Maintainers.txt](https://raw.githubusercontent.com/tianocore/edk2/master/Maintainers.txt)
