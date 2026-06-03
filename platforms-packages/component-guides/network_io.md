# Network IO

## Welcome to the Network IO Project

**Introduction to the Network-IO Project**

Please see [NetworkPkg Getting Started
Guide](../core-packages/networkpkg_getting_started_guide.md) for EDK II updates

| ![Network Layer](../../images/networklayer.jpg) |
| :---: |
| **Figure 1. UEFI Network layering** |

This is a sub-project of the EDK project. Beginning with the developer
snapshot 20060807, the UEFI network stack support has been added to the
EDK. The UEFI network stack drivers bring the support of TCP/IP
networking to EDK, with different drivers implementing different TCP/IP
protocols. The hierarchal layering of the drivers is shown at right.
(For further information about the UEFI network stack, read the chapter
20-24 of The UEFI specification 2.0 from
[UEFI.org](https://www.uefi.org))

To help develop networking applications, this sub-project provides a
SNPNT32 driver which implements the EFI_SIMPLE_NETWORK_PROTOCOL for the
NT32 platform. This driver in conjunction with the UEFI network stack
drivers can be used to develop and test networking applications on
Windows® NT operating system through the EDK’s NT32 platform emulation
environment.

The SNPNT32 driver depends on the WinPcap® to transmit and receive
packets on the Windows® system. To limit the number of symbols imported
into the NT32 platform, SnpNt32 call the functions in the SnpNt32Io
dynamic library to transmit/receive packets. The SnpNt32Io library in
turn consumes the service provided by WinPcap®. This sub-project
(Network-IO) produces the SnpNt32Io library.

The Network-IO project was released under the BSD license .

**Using the Network-IO**

The UEFI network stack getting started guide contains detailed
instructions to start to use the UEFI network stack on NT32 emulator.
You can find more information there.

Download the [UEFI Network Stack Getting Started
Guide(pdf)](https://sourceforge.net/projects/network-io/files/Documents/EFINetworkStackGettingStarted.pdf/download)
for guidance on building and using the library to enable UEFI network
stack under NT32 Platform Emulator

Download the [snapshot](https://github.com/tianocore/edk2-NetNt32Io) or
official release of the Network-IO project.

### Other Common Links and Downloads

- Directory of [Network-IO Source Code Snapshots (Development
  Versions)](https://github.com/tianocore/edk2-NetNt32Io)
- Directory of Network-IO Source Code Snapshots (Official Releases)
- Link to the TianoCore Community Project
- Link to the [UEFI2.0 Specification](https://www.uefi.org/)
- Download [WinPcap® library](https://www.winpcap.org/)
- Archives of the "Dev" Mailing List

**Intel Gig Undi**
Intel® Ethernet Connections Boot Utility, Preboot Images, and UEFI
Drivers -
[Download](https://downloadcenter.intel.com/download/19186/Intel-Ethernet-Connections-Boot-Utility-Preboot-Images-and-EFI-Drivers)
Latest Versions of the UEFI Gig Undi Bin and Source:

- Download preboot.exe and expand to disk
- Goto APPS\EFI\OPENSRC for undi source. Copy paste InteUndiPkg to edk2
  source directory and build

  ```
  Build -p IntelGigUndiPkg.dsc
  ```

- Undi binaries are in EFIx64

**Project Points of Contact**

- Network-IO Project Owner (China): Ruth Li, Intel Corporation,
  <ruth.li@intel.com>
