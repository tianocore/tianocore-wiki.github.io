# EDK II News

**NEW on this project is the MdePkg version 1.01 and Core Stable version
0.90 release.
([here](https://sourceforge.net/projects/edk2/files/Releases/EDK%20II%20Core%20(v%200.90)/))**

This is a further extension of the EDK II, from the MdePkg 1.00 Package
released June 14, 2009. This Package can be found in the EDK II SVN
repository as r9029. This version represents the next major milestone in
the EDK II development.

Also with this release, the online help system for the MdePkg version
1.01 is now working and available,
**[here](https://github.com/tianocore/edk2)**, for use directly from the
Open Source Community Website.(using [https://www.doxygen.org/](https://www.doxygen.org/))

The release contains the following components:

- MdePkg version 1.01 - updates to the MdePkg 1.00
- MdeModulePkg 0.90
- IntelFrameworkPkg 0.90
- IntelFrameworkModulePkg 0.90
- EdkCompatibilityPkg 0.90
- Basetools - most recent Basetools since the MdePkg 1.00 release

Each package comes with the sources, the documentation in CHM format,
and the documentation in HTML format. The release also includes release
notes providing more technical content regarding the entire EDKII Core
Stable Release and its components.

**NOTE**: There have been issues reported when attempting to use the CHM
package after download from the website. There is now a document "Notice
on Downloading CHM format Files" available to explain the issues and
solutions in detail.

Your feedback is critical to making EDK II a success! Please submit any
enhancements, defects, or requests through the [Project Tracker
tool](https://sourceforge.net/apps/trac/edk2/report).

**Notes on the MdePkg version 1.00 release**

This package contains the following significant features:

- PROTOCOLs/PPIs/GUIDs and related data declarations in MdePkg/Include
  directory. They correspond to UEFI2.0, UEFI 2.1 and/or PI1.0
  specifications published by the UEFI Forum and the EFI1.10
  specification published by Intel.
- Data declarations in MdePkg/Include/IndustryStandard directory support
  applicable industry standards.
- Public library classes definitions in MdePkg/Include/Library support
  module development.
- Library instances in MdePkg/Library directory implement library
  classes for module build and platform integration of different CPU
  architectures and module types.
- Data structure declarations conform to the specification of "PCD
  Infrastructure". .
- Build validation file of MdePkg/MdePkg.dsc lists the library instances
  under their supported CPU architectures and module types.

The MdePkg provides developers working on UEFI Applications, UEFI
Drivers, UEFI Operating System Loaders, UEFI diagnostics, and UEFI
Platform Initialization (PI) compliant silicon modules with a solid UEFI
and PI compliant foundation from which to develop their modules under
EDK II.

The MdePkg 1.00 also features the following quality metrics:

- The MdePkg source code follows EDK II Coding Standard, which inludes
  the use of Doxygen style file and function headers. The documentation
  generated from the MdePkg is available in both CHM and HTML formats.
- All the library source code can build without errors across several
  operating systems and tool chains
- All MDE library interfaces and PCD entries are covered and verified by
  MDE library test cases at API level for functionality and conformance.

Of particular note to developers: the r8508 of the entire EDK II project
contains the complete set of EDK II components used to test and validate
this release.

Also available are the Base Tools used to verify this package and
documentation of the MdePkg. The BaseTools package provides both
binaries build tools and source code files with configuration files to
build the MdePkg. It facilitates the most common development under
multiple operating systems. The help documentation is available both as
linked HTML and as comress HTML in CHM file format.
