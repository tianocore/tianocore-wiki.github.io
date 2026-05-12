# Udk2010 Sr1 Features

UDK2010
SR1 Feature Details

**Features Summary:**

- This release will support Microsoft® Windows® 8 Secure Boot feature.
  See [How to Enable Security](../../development/tutorials-howto/how_to_enable_security.md) for
  details
- Features added to support the UEFI 2.3.1 specification from the
  official [UEFI Forum Web](https://uefi.org) site

- EDK II Build tools and Updates to the
  [EDK II v1.22 specifications](../../reference/specs-standards/edk_ii_specifications.md)

<table width="500">

<tr>

<th colspan="2" style="background-color:#ff5c00">

Feature
Details

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#95b3d7">

Support Asynchronous Block Io

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#dbe5f1">

Secure Storage Protocol: enable
[Opal](https://www.trustedcomputinggroup.org/developers/storage)/[eDrive](https://msdn.microsoft.com/en-us/library/windows/hardware/br259095.aspx)
SATA devices using the EFI_STORAGE_SECURITY_COMMAND_PROTOCOL, ATA-8
Trusted Send/Receive and IEEE1667 Silo (UEFI 2.3.1a
specification)

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#95b3d7">

Networking Improvements:

- Erata related to Netboot6-DUID
- Provide more DHCP4 & DHCP6 API support

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#dbe5f1">

TCG Physical Presence (PP). Based on the Physical Presence Interface
Specification Version 1.20, Revision 1.0. More information at the
[Trusted Computing Group
website](https://www.trustedcomputinggroup.org/resources/tcg_physical_presence_interface_specification).

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#95b3d7">

USB 3.0 Controller Support (XHCI)

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#dbe5f1">

Update Internal Forms Representation (IFR) implementation to match UEFI
2.3.1 Specification.

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#95b3d7">

iSCSI (ip6) open source implementation for IPv6

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#dbe5f1">

Security Package (SecurityPkg). Open source package including the
following items:

- TPM 1.2 Measured Boot
- UEFI variable support for Secure Boot as defined by UEFI 2.3.1a
  (EFI_VARIABLE_TIME_BASED_AUTHENTICATED_WRITE_ACCESS attribute with
  EFI_VARIABLE_AUTHENTICATION_2 and EFI_VARIABLE_AUTHENTICATION support)
- DXE Image Verification library to support Secure Boot (per UEFI
  2.3.1a)
- User Identity Support (per UEFI 2.3.1a)
- PK x509 Certificate Support
- Support EFI_VARIABLE_AUTHENTICATION_2 for PK variable format (per UEFI
  2.3.1a)
- Updates for code to pass SecureTest package
- Add enable/disable mechanism for Secure Boot

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#95b3d7">

User Identification (UID) Errata from UEFI 2.3.1 Specification

</th>

</tr>

<tr>

<th colspan="1" style="background-color:#dbe5f1">

Updates to the EDK II Build Process.

- Support MACRO definitions in build tools.
- Updates for [EDK II v1.22 specifications](../../reference/specs-standards/edk_ii_specifications.md)
  (Build, DEC, DSC, FDF, INF)

</th>

</tr>

</table>
