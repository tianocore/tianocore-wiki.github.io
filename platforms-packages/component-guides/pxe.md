# Preboot Execution Environment (PXE)

PXE is a standardized client-server solution that boots an agent via network, allowing management tasks in the absence
of a running OS. EDK II has PXE Boot specification compliant implementation for UEFI. This feature is commonly referred
to as "PXE boot".

This page describes the operation of PXE. There are separate pages describing how to [Enable UEFI PXE Boot in EDK
II](../../development/tutorials-howto/enable_uefi_pxe_boot_in_edk_ii.md) and [Configuring PXE Boot Servers for
UEFI](../../development/tutorials-howto/configuring_pxe_boot_servers_for_uefi.md).

## History & Related Specifications

PXE was introduced as part of the Wired for Management Baseline (WfM) Specification by Intel Corporation in 1997. It was
described in a separate PXE 1.0 specification since Wired for Management 2.0. Later, the 2.1 update was published in
September 1999.

PXE 2.1 describes the IPv4-based network boot process. It does not cover IPv6-based PXE, but this is described in the
UEFI 2.2 specification. The UEFI 2.6 specification describes the IPv6-based PXE process in Section 23.3.1. The DHCP6
options used in PXE process are also described in the UEFI specification.

## Related Protocols

The UEFI specification introduces the following protocols related to PXE boot:

* PXE Base Code Protocol – provides several features to utilize the PXE-compatible devices, for network access and
  network booting.
* PXE Base Code Callback Protocol – provides callback function which will be invoked when the PXE Base Code Protocol is
  about to transmit, has received, or is waiting to receive a packet.
* Load File Protocol – loads the boot file to specified buffer, which allows the boot manger to boot the file later.

## Feature Scope

* Support PXE boot over IPv4 stack, IPv6 stack, or both
* Network Boot Options using PXE are automatically created
* Support PXE Redirection servers (Proxy DHCP servers)
* PXE Vendor Options support (option 43):
  * PXE Discovery Control (sub-opt 6, 7)
  * PXE Boot Servers (sub-opt 8)
  * PXE Boot Menu (sub-opt 9, 10)

## PXE Offer Types

There are eight offer types defined in PXEBC_OFFER_TYPE. All of them are supported in IPv4-based PXE. The rules for
distinguishing these offers include:

* If the offer is a pure DHCP packet, it is PxeOfferTypeDhcpOnly.
* If the offer does not have DHCP4 option 53, and it contains a DHCP option 67 provides boot file name, it is
  PxeOfferTypeBootp.
* If the offer has a DHCP4 option 60, or DHCP6 option 16, and the value is starting with “PXEClient”, it is a PXE offer.
* If the PXE offer has a zero “yiaddr” in the DHCP header, it is a proxy offer.
* If the offer has a valid discover vendor option, it is a PXE 1.0 offer.
* If the offer has a valid MTFTP vendor option, it is a WfM 1.1 offer.

The six remaining PXE offers are described below:

| | discover vendor option | MTFTP vendor option | N/A |
| --- | --- | --- | --- |
| “PXEClient” available  | PxeOfferTypeDhcpPxe10 | PxeOfferTypeDhcpWfm11a | PxeOfferTypeDhcpBinl&nbsp; |
| “PXEClient” available && Yiaddr == 0  | PxeOfferTypeProxyPxe10 | PxeOfferTypeProxyWfm11a | PxeOfferTypeProxyBinl&nbsp; |

Note that “Binl” is a term used in WfM specification and short for “Boot Intervention Network Layer; extended DHCP
service”.
The offers are selected according to a predefined policy. The priority of the offers is defined in that policy as
following:

1. PxeOfferTypeDhcpPxe10
2. PxeOfferTypeDhcpWfm11a
3. PxeOfferTypeProxyPxe10 + PxeOfferTypeDhcpOnly
4. PxeOfferTypeProxyWfm11a + PxeOfferTypeDhcpOnly
5. PxeOfferTypeDhcpBinl
6. PxeOfferTypeProxyBinl + PxeOfferTypeDhcpOnly
7. PxeOfferTypeDhcpOnly offer which contains DHCP option 67 for boot file name
8. PxeOfferTypeBootp + PxeOfferTypeDhcpOnly

In IPv6-based PXE, only 3 are supported (PxeOfferTypeDhcpBinl, PxeOfferTypeProxyBinl, PxeOfferTypeDhcpOnly).

## PXE DHCP Timeout

In IPv4-based PXE, DHCP discovery will be retried four times. The four timeouts are 4, 8, 16 and 32 seconds
respectively, to compliant with PXE 2.1 specification. UEFI specification does not define the timeouts for IPv6-based
PXE. In current EDKII implementation, DHCPv6 solicit will be retried four times also. The initial retransmission timeout
is 4 seconds and maximum retransmission timeout for each retry is 32 seconds. PXE client should wait for the timeout
then select most preferred offer among all the received offers.

## PXE Boot Process

This section will briefly introduce the PXE boot process, please refer the section 2.2 of PXE v2.1 specification for a
detailed step-by-step synopsis of the PXE protocol.
![PXE Boot](../../Projects/NetworkPkg/images/PXE_Boot.png "PXE BOOT (PXE
Spec V2.1 Figure 2-1)")
The picture shows a typical IPv4 PXE boot flow (it's from PXE Spec V2.1 Figure 2-1).

**Step 1-4** is DHCP protocol with several extended DHCP option tags. The client should broadcast a DHCP Discover
message with "PXEClient" extension tags to trigger the DHCP process. Then it should select offers, get the address
configuration and boot path information, and complete the standard DHCP protocol by sending a request for the selected
address to the server and waiting for the Ack. It might also need to perform DNS resolution to translate the server's
host address to IP address.

**Step 5-6** takes place between the client and a Boot Server. The client should select and discover a Boot Server from
the obtained server list in step 1-4. This phase is not a part of standard DHCP protocol, but uses the DHCP Request and
Ack message format as a convenient for communication. The client should send the request message to port 67 (broadcast)
or port 4011 (multicast/unicast) of the selected boot server, and wait a DHCP ack for the boot file name and MTFTP
configuration parameters.

**Step 7-9** is the downloading of the network bootstrap program (NBP). The client will load the NBP into the
computer’s local memory using TFTP, verify the image and execute it finally.

* In a Windows Deployment Services (WDS) environment, the NBP is provided as wdsmgfw.efi.
* In a Linux environment, the NBP is provided by UEFI-enabled boot loaders such as GRUB, GRUB2 or ELILO.

Take UEFI PXE with Microsoft WDS as example, the NBP would continue to download several files to client platform. After
the downloading finished, the WDS loader calls ExitBootService() and transits to Runtime phase. The OS kernel starts
execution and takes over the control to the system. The OS network stack is also started for handling network
operations.

Please refer to [UEFI PXE Boot Performance
Analysis](https://software.intel.com/sites/default/files/managed/2d/04/intel-uefi-pxe-boot-performance-analysis.pdf) for
more details.

## PXE Boot Verification

Operating systems used to verify EDK II PXE Boot functionality:

* Microsoft Windows 10/8.1/7
* SUSE Linux Enterprise 11 (SP3)
* Red Hat Enterprise Linux 7.0

## PXE Limitations

* PXE uses UDP as transport layer protocol. TCP is not supported.
* PXE is designed to work within a corporate network, not outside of a company firewall.
* PXE uses TFTP and does not support a secure transport method (ex: HTTPS).

## References

1. PXE 2.1 specification:
[https://download.intel.com/design/archives/wfm/downloads/pxespec.pdf](https://download.intel.com/design/archives/wfm/downloads/pxespec.pdf)
2. UEFI 2.6 specification:
[https://www.uefi.org/sites/default/files/resources/UEFI%20Spec%202_6.pdf](https://www.uefi.org/sites/default/files/resources/UEFI%20Spec%202_6.pdf)
3. WfM2.0 specification:
[https://download.intel.com/design/archives/wfm/downloads/base20.pdf](https://download.intel.com/design/archives/wfm/downloads/base20.pdf)
4. UEFI PXE Boot Performance Analysis:
[https://software.intel.com/sites/default/files/managed/2d/04/intel-uefi-pxe-boot-performance-analysis.pdf](https://software.intel.com/sites/default/files/managed/2d/04/intel-uefi-pxe-boot-performance-analysis.pdf)
5. NetworkPkg Getting Started Guide:
[NetworkPkg Getting Started Guide](../core-packages/networkpkg_getting_started_guide.md)
