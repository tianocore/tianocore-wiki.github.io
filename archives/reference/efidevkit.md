# EFI Dev Kit

```admonish caution "Deprecated Project"
[EDK II](EDK_II) for the
latest information. This page is maintained for historical purposes and
will be archived in a future site update.
```

The **EDK** is the open-source component of the "Framework", Intel's
implementation of the EFI Specification, which was developed under the
project codenamed "Tiano". The EDK was released under the
[BSD
License](../../reference/legal-licenses/bsd_license.md). Refer to the
[EDK
section of the EFI and Framework Open Source FAQs](edk.md) for a full
explanation of how the EDK relates to the EFI effort.

## Working with EDK

The EDK is essentially a container for the Framework's Foundation code
and sample drivers. The EDK is also a development kit for developing,
debugging, and testing EFI and Framework drivers, EFI Option ROMs, and
EFI Applications for use in the Framework environment. To download
snapshots of development or official release of the EDK, click on the
link and choose the release to start the download process. Download the
[EDK Getting Started
Guide](https://sourceforge.net/projects/efidevkit/files/Edk%20Getting%20Started%20Guide%5B1%5D.0.41.pdf/download)
(pdf) for guidance on building and using the EDK to develop and test
drivers. The [Framework
Specification](https://www.intel.com/technology/framework/spec.htm) on
the Intel website. To learn more about the EDK codebase see the [EDK
Module Structure
Explained](https://sourceforge.net/apps/mediawiki/tianocore/index.php?title=Modules).

- Developer page: [https://sourceforge.net/projects/efidevkit/develop](https://sourceforge.net/projects/efidevkit/develop)
- EDK Files page: [https://sourceforge.net/projects/efidevkit/files](https://sourceforge.net/projects/efidevkit/files)

### EDK Releases

#### Edk 1.06.zip

- **Download**: [Edk 1.06.zip](https://sourceforge.net/projects/efidevkit/files/Releases/Official%20Releases/Edk%201.06.zip/download)
- **What is it?**: Contains UEFI 2.1, Framework 0.9, PI 1.0 and EFI shell: core and sample driver source code
  for EDK build source code environment and directory structure.
- **EDK Shell**: [EfiShell 1.06.zip](https://sourceforge.net/projects/efi-shell/files/Releases/Official%20Releases/EfiShell%201.06.zip/download)
- **Documents**:
  - ShellCommandManual.pdf
  - Glue_Library.pdf
  - EDK BuildDocument.pdf
  - EBC Debugger User Manual.pdf
  - EfiDriverLib.pdf
  - EdkReference Manual.pdf
  - EdkReferenceManual.zip
  - VFR.pdf
  - Edk Getting Started Guide\[1\].pdf
  - EbcDegubber Release notes
  - DuetRelNotes.txt

### Mobile / Client Patches

#### Client Framework Patch 9

- **Downloads**:
  - [Client Framework Patch 9](https://sourceforge.net/projects/efidevkit/files/Releases/Others/Other%20Contribution/EDK1117_Client_Framework_PatchV9.zip/download)
  - [Previous Releases](https://sourceforge.net/projects/efidevkit/files/Releases/Others/Other%20Contribution/)
- **Description**: Intel Green H - EDK1117 Patch Version 9

This package contains the full set of Green H files intended to be used for the *Broadwell Platform* and
*Bay Trail Platform* (Intel Corporation Code names). The configuration that will be utilized for platform
validation is UEFI 2.3.1 and Framework 0.9x (without Framework HII). This package does not contain the
associated build tools required for UEFI 2.3.1 support.

At a high level, this update requires a codebase owner to:

1. Integrate this Green H package
2. Integrate the latest Broadwell Platform or Bay Trail platform framework reference code packages
3. Integrate the latest open source tools to support UEFI 2.3.1 HII
4. Update the SetupBrowser and HII producer and consumer modules and tools to support UEFI 2.3.1

The patch should function in either UEFI 2.0 or UEFI 2.3.1 modes, but UEFI 2.3.1 will be the only
configuration used by Intel validation. It is recommended that the UEFI 2.3.1 conversion be made as quickly
and completely as possible for the 2014 platforms.

Please refer to EDK1117_PatchV9_Details.txt for the changes from the open source EDK 11/17/2006 AKA [EDK Release 1.01](https://sourceforge.net/projects/efidevkit/files/Releases/Official%20Releases/EDK%201.01.zip/download).

Further details are available from your Intel client BIOS support representative.

### Development Snapshots

- [Edk-Dev Shapshot-20100527.zip](https://sourceforge.net/projects/efidevkit/files/Releases/Development%20Snapshots/Edk-Dev-Snapshot-20100527.zip/download)
- [Development Snapshot Diff Patches](https://sourceforge.net/projects/efidevkit/files/Releases/Development%20Snapshots%20Diff%20Patches/)
  (To see the differences between snapshot versions)

### Other Projects

- [EDK HII Evaluation Package](https://sourceforge.net/projects/efidevkit/files/Releases/Others/EDK%20HII%20Evaluation%20Package.zip/download)
- [Intel Undi UEFI Source 4606.zip](https://sourceforge.net/projects/efidevkit/files/Releases/Others/Other%20Contribution/gig_efi_src_4606.zip/download)
- [Intel Undi UEFI Rel 15 7 Src.zip](https://sourceforge.net/projects/efidevkit/files/Releases/Others/Other%20Contribution/Intel_Undi_UEFI_Rel_15_7_src.zip/download)
- [GOP Release.zip](https://sourceforge.net/projects/efidevkit/files/Releases/Others/Other%20Contribution/GOP_Release.zip/download)
- [ixgbe.src.2201.zip](https://sourceforge.net/projects/efidevkit/files/Releases/Others/Other%20Contribution/ixgbe_src_2201.zip/download)
- [gig pcie src 4109.zip](https://sourceforge.net/projects/efidevkit/files/Releases/Others/Other%20Contribution/gig_pcie_src_4109.zip/download)
- [gig pci src 3512.zip](https://sourceforge.net/projects/efidevkit/files/Releases/Others/Other%20Contribution/gig_pci_src_3512.zip/download)

**Description**: Other Projects - Fix to the Undi EDK Intel(R) PRO/1000 Network Driver

[Previous EDK and Shell
Releases](../../releases-history/archives/previous_edk_and_shell_releases.md)

## Sub Projects

[edk-apps](edk_apps.md)

[network-io](../../platforms-packages/component-guides/network_io.md)

License information:
[BSD](https://www.opensource.org/licenses/bsd-license.php)

Project owner(s): ,

## Member Links

- [Query of Open and Active EDK
  Defects](https://sourceforge.net/apps/trac/efidevkit/report/8)
- [Request for a Project
  Role](https://sourceforge.net/project/memberlist.php?group_id=288624)
  (initial or promotion)

## Project Points of Contact

- [EDK Module
  Owners](../../development/design-architecture/modules.md)

## Useful Links

- [Link to the Intel EFI 1.10
  Specification](https://developer.intel.com/technology/efi/index.htm)
  (on Intel's Site)
