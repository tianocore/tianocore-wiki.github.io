# Configuring PXE Boot Servers for UEFI

This page collects resources for configuring [PXE](../../platforms-packages/component-guides/pxe.md) servers to boot
UEFI images. The defacto PXE configuration is typically setup for 16-bit x86 legacy BIOS images, so adding UEFI support
requires changes to server config files.

You can also refer to Intel's [UEFI PXE Boot Performance
Analysis](https://software.intel.com/sites/default/files/managed/2d/04/intel-uefi-pxe-boot-performance-analysis.pdf)
whitepaper for an overfoew fo the UEFI PXE boot process, and tips for optimizing boot time on Microsoft Windows and
Linux platforms.

## Linux

## Fedora

[Setting Up an Installation Server](https://docs.fedoraproject.org/en-US/Fedora/19/html/Installation_Guide/ap-install-server.html)

[Providing and configuring bootloaders for PXE clients](https://docs.fedoraproject.org/en-US/Fedora/24/html/Installation_Guide/pxe-bootloader.html)

[Configuring for EFI](https://docs.fedoraproject.org/en-US/Fedora/19/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

## RedHat

[Setting Up an Installation Server](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/ap-install-server.html)

[Configuring PXE Boot for EFI](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Installation_Guide/s1-netboot-pxe-config-efi.html)

## SUSE Linux

[SUSE® Linux Enterprise Server 11 SP2 for UEFI Clients, Best Practices White Paper](https://www.suse.com/docrep/documents/6ytjkpvom0/sles_11_sp2_for_uefi_client_best_practices_white_paper.pdf)

[Using PXE with UEFI on PowerEdge Servers with SUSE Linux Enterprise Server 11](https://linux.dell.com/files/whitepapers/PXE_UEFI_Dell_SLES11_20oct2010.pdf)

[Deployment Guide - SUSE Linux Enterprise Desktop 12 SP3 (Sections 6.1-6.3 )](https://www.suse.com/documentation/sled-12/pdfdoc/book_sle_deployment/book_sle_deployment.pdf)

[How to Set Up a Multi-PXE Installation Server (Setting Up DHCP Services, -Sec 4,7 )](https://www.suse.com/documentation/suse-best-practices/singlehtml/sbp-multi-pxe-install/sbp-multi-pxe-install.html#sec.setup_dhcp)

## Ubuntu

[UEFI Overview](https://wiki.ubuntu.com/UEFI/)

[UEFI PXE netboot / install procedure](https://wiki.ubuntu.com/UEFI/PXE-netboot-install)

[SecureBoot-compatible UEFI netboot over IPv4 and IPv6](https://wiki.ubuntu.com/UEFI/SecureBoot-PXE-IPv6)

## Microsoft Windows

[Windows 10 deployment scenarios](https://docs.microsoft.com/en-us/windows/deployment/windows-10-deployment-scenarios)

[Windows Deployment Services (WDS) support for UEFI](https://support.microsoft.com/en-us/help/2938884/windows-deployment-services-wds-support-for-uefi)

[Deploy Windows 10 using PXE and Configuration Manager](https://docs.microsoft.com/en-us/windows/deployment/deploy-windows-sccm/deploy-windows-10-using-pxe-and-configuration-manager)

[Configure a PXE server to load Windows PE](https://docs.microsoft.com/en-us/windows/deployment/configure-a-pxe-server-to-load-windows-pe)

## iPXE

[iPXE](https://ipxe.org/start) is an open source network boot firmware implementation. iPXE does not rely on the EDK II
Network Stack, but offers many similar functions.

[iPXE installation and EFI](https://doc.rogerwhittaker.org.uk/ipxe-installation-and-EFI/)

[iPXE - UEFI HTTP chainloading](https://ipxe.org/appnote/uefihttp)
