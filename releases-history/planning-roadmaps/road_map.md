# Roadmap

UDK2010 roadmap for Tianocore.org feature details.

- Look for the upcoming release available **Now**
- Release UDK2010.SR1
- Supports Microsoft® Windows® 8 Secure Boot
- Adds features from the UEFI 2.3.1 specification
- Updates EDK II build tools and EDK II specifications

## Feature Details

### Support Asynchronous Block IO

Support for asynchronous block IO operations.

### Secure Storage Protocol

Enable [Opal](https://www.trustedcomputinggroup.org/developers/storage) and
[eDrive](https://msdn.microsoft.com/en-us/library/windows/hardware/br259095.aspx)
SATA devices using the EFI_STORAGE_SECURITY_COMMAND_PROTOCOL, ATA-8 Trusted
Send/Receive, and IEEE1667 Silo (UEFI 2.3.1a specification).

### Networking Improvements

- Errata related to Netboot6-DUID
- Additional DHCP4 and DHCP6 API support

### TCG Physical Presence (PP)

Based on the Physical Presence Interface Specification Version 1.20, Revision
1.0. More information at the
[Trusted Computing Group website](https://www.trustedcomputinggroup.org/resources/tcg_physical_presence_interface_specification).

### USB 3.0 Controller Support (XHCI)

Adds USB 3.0 controller support.

### Internal Forms Representation (IFR)

Updates IFR implementation to match the UEFI 2.3.1 specification.

### Security Package (SecurityPkg)

Open source package including:

- TPM 1.2 Measured Boot
- UEFI variable support for Secure Boot as defined by UEFI 2.3.1a
  (EFI_VARIABLE_TIME_BASED_AUTHENTICATED_WRITE_ACCESS with
  EFI_VARIABLE_AUTHENTICATION_2 and EFI_VARIABLE_AUTHENTICATION support)
- DXE Image Verification library to support Secure Boot (per UEFI 2.3.1a)
- User Identity Support (per UEFI 2.3.1a)
- PK x509 Certificate Support
- Support EFI_VARIABLE_AUTHENTICATION_2 for PK variable format (per UEFI
  2.3.1a)
- Updates for code to pass SecureTest package
- Enable/disable mechanism for Secure Boot

### User Identification (UID) Errata

Addresses UID errata from the UEFI 2.3.1 specification.

### EDK II Build Process Updates

- Support MACRO definitions in build tools
- Updates for
  [EDK II v1.22 specifications](../../reference/specs-standards/edk_ii_specifications.md)
  (Build, DEC, DSC, FDF, INF). See the
  [download](../../reference/specs-standards/edk_ii_specifications.md).
