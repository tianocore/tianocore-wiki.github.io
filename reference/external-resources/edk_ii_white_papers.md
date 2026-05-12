# EDK II White Papers

This page contains a list of White papers and technical documents in
chronological order
Check [EDK II Security White Papers](../../security/tools-practices/edk_ii_security_white_papers.md)
for documents on security

<table>

<tr>

<th colspan="4" style="background-color:#a9c6dd">

EDK II White papers

</th>

</tr>

<tr>

<td>

**Download PDF**

</td>

<td>

**Title**
**Description**
\***Date Published**

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore/edk2-platforms/blob/devel-MinPlatform/Platform/Intel/MinPlatformPkg/Docs/A_Tour_Beyond_BIOS_Open_Source_IA_Firmware_Platform_Design_Guide_in_EFI_Developer_Kit_II%20-%20V2.pdf)

</td>

<td>

[A Tour Beyond BIOS Open Source IA Firmware Platform Design Guide in EDK
II -
V2](https://github.com/tianocore/edk2-platforms/blob/devel-MinPlatform/Platform/Intel/MinPlatformPkg/Docs/A_Tour_Beyond_BIOS_Open_Source_IA_Firmware_Platform_Design_Guide_in_EFI_Developer_Kit_II%20-%20V2.pdf)-
contributed by Vincent Zimmer, Jiewen Yao, Michael Kubacki, Amy Chan,
Rangasai Chaganty and Chasel Chiu

This paper introduces a design guide for an EDK II open source IA
firmware solution. In order to make an open IA firmware solution simple,
we demonstrate a firmware design approach with minimal features.
The only criteria are

1. It can boot to the OS
2. It is secure.

We can remove many unnecessary silicon or platform features like Capsule
update, Recovery, S3 resume, SMBIOS, EC, Super IO (SIO), I2C, and only
enable ACPI & SMM to support booting.

- May 2018 [V2
  .PDF](https://github.com/tianocore/edk2-platforms/blob/devel-MinPlatform/Platform/Intel/MinPlatformPkg/Docs/A_Tour_Beyond_BIOS_Open_Source_IA_Firmware_Platform_Design_Guide_in_EFI_Developer_Kit_II%20-%20V2.pdf)
- May 2016
  [.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Open_Source_IA_Firmware_Platform_Design_Guide_in_EFI_Developer_Kit_II.pdf)

</td>

</tr>

<tr>

<td>

[HTML](https://tianocore-docs.github.io/ATBB-Mitigate_Buffer_Overflow_in_UEFI/draft/)
[PDF](https://tianocore-docs.github.io/ATBB-Mitigate_Buffer_Overflow_in_UEFI/draft/ATBB-Mitigate_Buffer_Overflow_in_UEFI-draft.pdf)
[MOBI](https://tianocore-docs.github.io/ATBB-Mitigate_Buffer_Overflow_in_UEFI/draft/ATBB-Mitigate_Buffer_Overflow_in_UEFI-draft.mobi)
[EPUB](https://tianocore-docs.github.io/ATBB-Mitigate_Buffer_Overflow_in_UEFI/draft/ATBB-Mitigate_Buffer_Overflow_in_UEFI-draft.epub)

</td>

<td>

[A Tour Beyond BIOS- Security Enhancement to Mitigate Buffer Overflow in
UEFI](https://tianocore-docs.github.io/ATBB-Mitigate_Buffer_Overflow_in_UEFI/draft/)
contributed by Jiewen Yao, Vincent Zimmer and Jian Wang

A buffer overflow is “one of the most important exploitation techniques
in the history of computer security.” \[
[Tanenbaum](https://www.amazon.com/s/ref=dp_byline_sr_book_1?ie=UTF8&field-author=Andrew+S.+Tanenbaum&search-alias=books&text=Andrew+S.+Tanenbaum&sort=relevancerank)
\] “Buffer overflows are ideally suited for introducing three of the
most important protection mechanisms available in most modern systems:
stack canaries, data execution protection, and address-space layout
randomization.”\[
[Tanenbaum](https://www.amazon.com/s/ref=dp_byline_sr_book_1?ie=UTF8&field-author=Andrew+S.+Tanenbaum&search-alias=books&text=Andrew+S.+Tanenbaum&sort=relevancerank)
\] However, the current UEFI firmware implementation only adopted a few
of these mechanisms.
This paper will introduce how to enable the protection mechanisms in
UEFI firmware to harden the pre-boot phase.

- March 2018 [V 2.0
  HTML](https://tianocore-docs.github.io/ATBB-Mitigate_Buffer_Overflow_in_UEFI/draft/)
- October 2016 [V 1.0
  PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Securiy_Enhancement_to_Mitigate_Buffer_Overflow_in_UEFI.pdf)

</td>

</tr>

<tr>

<td>

[HTML](https://tianocore-docs.github.io/EDKIIHttpBootGettingStartedGuide/draft/)
[PDF](https://tianocore-docs.github.io/EDKIIHttpBootGettingStartedGuide/draft/EDKIIHttpBootGettingStartedGuide-draft.pdf)
[MOBI](https://tianocore-docs.github.io/EDKIIHttpBootGettingStartedGuide/draft/EDKIIHttpBootGettingStartedGuide-draft.mobi)
[EPUB](https://tianocore-docs.github.io/EDKIIHttpBootGettingStartedGuide/draft/EDKIIHttpBootGettingStartedGuide-draft.epub)

</td>

<td>

[EDK II HTTP Boot Getting Started
Guide](https://tianocore-docs.github.io/EDKIIHttpBootGettingStartedGuide/draft/)-
contributed by Ye Ting, Fu Siyuan, and Zhang Lubo
This document is a getting started guide for using the HTTP boot
capability introduced in the UEFI Specification, revision 2.5.

- January 2018 Rev 0.9
- April 2016 Rev 0.8
  [.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/EDKIIHttpBootGettingStartedGuide_0_8.pdf)

</td>

</tr>

<tr>

<td>

[HTML](https://tianocore-docs.github.io/EDKIIHttpsBootGettingStartedGuide/draft/)
[PDF](https://tianocore-docs.github.io/EDKIIHttpsBootGettingStartedGuide/draft/EDKIIHttpsBootGettingStartedGuide-draft.pdf)
[MOBI](https://tianocore-docs.github.io/EDKIIHttpsBootGettingStartedGuide/draft/EDKIIHttpsBootGettingStartedGuide-draft.mobi)
[EPUB](https://tianocore-docs.github.io/EDKIIHttpsBootGettingStartedGuide/draft/EDKIIHttpsBootGettingStartedGuide-draft.epub)

</td>

<td>

[Getting Started with UEFI HTTPS Boot on EDK
II](https://tianocore-docs.github.io/EDKIIHttpsBootGettingStartedGuide/draft/)
contributed by Wu Jiaxin, Fu Siyuan and Brian Richardson
HTTP over TLS (HTTPS) boot is a standard implementation for securely
booting using the Unified Extensible Firmware Interface (UEFI) over a
network device. HTTPS Boot is especially important for clients using
potentially insecure networks outside of corporate infrastructure.
Security for UEFI HTTPS Boot is provided by the underlying Transport
Layer Security (TLS). This paper assumes the reader is familiar with the
[EDK II HTTP Boot Getting Started
Guide](https://tianocore-docs.github.io/EDKIIHttpBootGettingStartedGuide/draft/)

`available on this page.`

- May 2017 Rev 1.3
- Feb 2017 Rev 1.2
  [.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/EDKIIHttpsBootGettingStartedGuide_1.2.pdf)

</td>

</tr>

<tr>

<td>

[Gitbook
PDF](https://edk2-docs.gitbook.io/a-tour-beyond-bios-memory-protection-in-uefi-bios)

</td>

<td>

[A Tour Beyond BIOS- Memory Protection in UEFI
BIOS](https://edk2-docs.gitbook.io/a-tour-beyond-bios-memory-protection-in-uefi-bios)
contributed by Jiewen Yao and Vincent Zimmer
Data execution protection (DEP) is intended to prevent an application or
service from executing code from a non-executable memory region. This
helps prevent certain exploits that store code via a buffer overflow.

In the White paper \[
[https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Securiy_Enhancement_to_Mitigate_Buffer_Overflow_in_UEFI.pdf](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Securiy_Enhancement_to_Mitigate_Buffer_Overflow_in_UEFI.pdf)
A Tour Beyond BIOS-Security Enhancement to Mitigate Buffer Overflow\]
below, we only discussed the DEP for protecting the stack and setting
the not-present page for detecting \`NULL\` address accesses and as the
guard page. This document will have a more comprehensive discussion of
the DEP adoption in the current UEFI firmware to harden the pre-boot
phase.

- March 2017

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Capsule_Update_and_Recovery_in_EDK_II.pdf)

</td>

<td>

[A Tour Beyond BIOS- Capsule Update and Recovery in EDK
II](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Capsule_Update_and_Recovery_in_EDK_II.pdf)
contributed by Jiewen Yao and Vincent Zimmer
The firmware update capability represents an important feature for the
system firmware on the mother board and the various device firmware
instances, such as a host bus adapter-attached PCI option ROM, embedded
controller (EC), baseboard management controller (BMC), etc. The
firmware recovery is also a feature to support firmware boot in recovery
mode in cases where the main flash image is errant or corrupt.
In this paper, we provide more details on how we implement capsule
update and recovery in EDK II.

- December 2016

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Security_Design_Guide_in_EDK_II.pdf)

</td>

<td>

[A Tour Beyond BIOS Security Design Guide in EDK
II](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Security_Design_Guide_in_EDK_II.pdf)
contributed by Jiewen Yao and Vincent Zimmer

The purpose of this document is to provide security guidelines to
developers, implementers, and code reviewers of the EDK II firmware. The
topics discussed in this paper are intended to aid in reducing bugs
associated with common security vulnerability classes present in EDK II.
Following these guidelines will increase the overall security of
platforms implementing the firmware and ensure platforms are not as
susceptible to malicious behavior.

- September 2016

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_Profiling_in_EDK_II.pdf)

</td>

<td>

[A Tour Beyond BIOS Implementing Profiling in EDK
II](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_Profiling_in_EDK_II.pdf)
-
contributed by Jiewen Yao, Vincent Zimmer, Star Zeng and Fan Jeff
The Unified Extensible Firmware Interface (UEFI) and Platform
Initialization (PI) specification defines rich execution environments
such as Security (SEC), Pre-EFI Initialization (PEI), Driver Execution
Environment (DXE), System Management Mode (SMM) and UEFI Runtime (RT)
for firmware drivers. There are more and more features added into a
firmware. At same time, the firmware still has a resource constrained
environment. In addition to functionality, the size, performance, and
security are three major concerns of a firmware engineer. This paper
introduces several profiling features implemented in EDK II to help the
UEFI firmware developer to analyze the size, performance and security of
a UEFI firmware implementation.

- July 2016

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Secure_SMM_Communication.pdf)

</td>

<td>

[A Tour Beyond BIOS Secure SMM
Communication](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Secure_SMM_Communication.pdf)-
contributed by Star Zeng, Vincent Zimmer and Jiewen Yao

This paper introduces how we can do secure SMM communication in a UEFI
BIOS.
Audience: This paper assumes that audience has basic EDKII/UEFI firmware
development experience, and basic knowledge of SMM.

- April 2016

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Memory_Map_And_Practices_in_UEFI_BIOS_V2.pdf)

</td>

<td>

[A Tour Beyond BIOS Memory Map and Practices in UEFI
BIOS](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Memory_Map_And_Practices_in_UEFI_BIOS_V2.pdf)-
contributed by Vincent Zimmer and Jiewen Yao

This paper introduces the memory map security practices in UEFI BIOS.
Audience: This paper assumes that audience has basic EDKII/UEFI firmware
development experience.

The main job of BIOS is to initialize the platform hardware and report
information to a generic operating system (OS). The memory map is one of
the most important pieces of information. The operating system can only
load a kernel, driver or application in the right place if it knows how
memory is allocated. In **UEFI Memory Map**, we introduced the memory
map design in UEFI BIOS, and saw how a typical platform reports the
memory map to an OS. In this paper we will discuss how to enhance the
memory map reporting and provide security practice for memory protection
to harden platforms.

- March 2016

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/EDK_II_Topology_-_SMM.pdf)
or
[.VSD](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/EDK_II_Topology_-_SMM.vsd)

</td>

<td>

[EDK II Topology
SMM](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/EDK_II_Topology_-_SMM.pdf) -
White Paper contributed by Lee Hamel
EDK II Topology – SMM: Topology of how SMM is set up and executed

- Jan 2016

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/EDK_II_Topology_-_S3.pdf)
or
[.VSD](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/EDK_II_Topology_-_S3.vsd)

</td>

<td>

[EDK II Topology
S3](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/EDK_II_Topology_-_S3.pdf) -
White Paper contributed by Lee Hamel
EDK II Topology - S3: Topology of how S3 is set up and executed

- Jan 2016

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/EDK_II_Topology_-_PCI_Enumeration.pdf)
or
[.VSD](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/EDK_II_Topology_-_PCI_Enumeration.vsd)

</td>

<td>

[EDK II Topology PCI
Enumeration](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/EDK_II_Topology_-_PCI_Enumeration.pdf)
-
White Paper contributed by Lee Hamel
EDK II Topology - PCI Enumeration: Topology of how PCI Enumeration is
set up and executed

- Jan 2016

</td>

</tr>

<tr>

<td>

[PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/UDK_Build_Integration_of_Reset_Vector.pdf)

</td>

<td>

[UDK Build Integration of Reset
Vector](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/Edk_II_Topology/UDK_Build_Integration_of_Reset_Vector.pdf)
-
White Paper contributed by Lee Hamel
How the Reset Vector is integrated into a UDK build

- Jan 2016

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_UEFI_Authenticated_Variables_in_SMM_with_EDKII_V2.pdf)

</td>

<td>

[A Tour Beyond BIOS Implementing UEFI Authenticated Variables in SMM
with
EDKII](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_UEFI_Authenticated_Variables_in_SMM_with_EDKII_V2.pdf)
This paper presents the internal structure and boot flow of the
SMM-based UEFI Authenticated Variable driver in the MDE Module Package
and Security Package of the EDKII.
Prerequisite This paper assumes that audience has EDKII/UEFI firmware
development experience. He or she should also be familiar with UEFI/PI
firmware infrastructure, such as SEC, PEI, DXE, runtime phase.

- Oct 2015

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_S3_resume_with_EDKII_V2.pdf)

</td>

<td>

[A Tour Beyond BIOS Implementing S3 Resume with
EDKII](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_S3_resume_with_EDKII_V2.pdf)
This paper presents the internal structure and boot flow of PI S3 resume
design, as implemented in the EDKII.

Prerequisite This paper assumes that audience has EDKII/UEFI firmware
development experience. He or she should also be familiar with
UEFI/PI/ACPI firmware infrastructure, such as SEC, PEI, DXE, runtime
phase, and S-states.

- Oct 2015

</td>

</tr>

<tr>

<td>

[PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_into_UEFI_Secure_Boot_White_Paper.pdf)
or
[Zip](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_into_UEFI_Secure_Boot_White_Paper.zip)

</td>

<td>

[A Tour Beyond BIOS into UEFI Secure Boot White
Paper](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/A_Tour_Beyond_BIOS_into_UEFI_Secure_Boot_White_Paper.pdf)
This document provides an overview of the implementation and intent
behind the UEFI Secure Boot feature and capability of UEFI
Specification, Version 2.3.1C, [https://www.uefi.org](https://www.uefi.org)

The goal of the paper is to provide

- an understanding of the motivations behind this capability
- a walk-through of the implementation
- a future evolution.

This paper targets firmware, software, and BIOS engineers.

- July 2012

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/EDK_II_Build_Decoded.pdf)

</td>

<td>

[EDK II Build
Decoded](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/EDK_II_Build_Decoded.pdf)
Discussion of the files that are used in a build and their purpose.

- April 2012

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/How-to-Create_VS_Solution_for_Existing_Code.pdf)

</td>

<td>

[How to create Visual Studio
solution](https://github.com/tianocore-docs/Docs/raw/main/White_Papers/How-to-Create_VS_Solution_for_Existing_Code.pdf)
How to create a Visual Studio solution for an EDK II tree.

- April 2012

</td>

</tr>

<tr>

<td>

[.PDF](https://github.com/tianocore-docs/Docs/raw/main/User_Docs/PerformanceOptimization_1_0.pdf)

</td>

<td>

[EDK II Performance
Optimization](https://github.com/tianocore-docs/Docs/raw/main/User_Docs/PerformanceOptimization_1_0.pdf)
This paper focuses on techniques and methodologies which can be used to
characterize and optimize the performance of EDK II based firmware.
(PDF)

- April 2010

</td>

</tr>

</table>
