# NetworkPkg

NetworkPkg provides IPv6 network stack drivers, IPsec driver,
[PXE](../component-guides/pxe.md) driver, iSCSI driver
and necessary shell applications for network configuration

More information: [NetworkPkg Getting Started
Guide](networkpkg_getting_started_guide.md)

| Network Package Details [Readme_NetworkPkg.txt](https://github.com/tianocore-docs/Docs/blob/main/User_Docs/Readme_NetworkPkg.txt) |
| --- |
| 1\. IPv6 network stack - NetworkPkg/Ip6Dxe - Ip6 driver, which produces EFI_IP6_PROTOCOL EFI_IP6_CONFIG_PROTOCOL - NetworkPkg/Udp6Dxe Udp6 driver, which produces EFI_UDP6_PROTOCOL - NetworkPkg/TcpDxe - TCP combo driver, which produces EFI_TCP4_PROTOCOL EFI_TCP6_PROTOCOL - NetworkPkg/Dhcp6Dxe - DHCP6 driver, which produces EFI_DHCP6_PROTOCOL - NetworkPkg/Mtftp6Dxe - MTFTP6 driver, which produces EFI_MTFTP6_PROTOCOL |
| 2\. IPsec - NetworkPkg/IpSecDxe - Ipsec driver, which produces EFI_IPSEC2_PROTOCOL EFI_IPSEC_CONFIG_PROTOCOL |
| 3\. PXE - NetworkPkg/UefiPxeBcDxe - PXE combo driver, which produces EFI_PXE_BASE_CODE_PROTOCOL EFI_LOAD_FILE_PROTOCOL |
| 4\. iSCSI - NetworkPkg/IScsiDxe - iSCSi driver, which produces: EFI_ISCSI_INITIATOR_NAME_PROTOCOL EFI_EXT_SCSI_PASS_THRU_PROTOCOL EFI_AUTHENTICATION_INFO_PROTOCOL |
| 5\. Shell Applications 1\) NetworkPkg/Application/Ping6 - This command is used to ping the target host with IPv6 stack. 2\) NetworkPkg/Application/IfConfig6 - This command is used to configure the default IP address of the UEFI IPv6. 3\) NetworkPkg/Application/IpSecConfig - This command is used to create, edit and delete the IPsec policy, i.e. Security Policy Database (SPD), Security Association Database (SAD), and Peer Authentication Database (PAD). 4\) NetworkPkg/Application/VConfig - This command is used to configure VLAN of a network interface. |

Note: UNDI, SNP, DPC and MNP drivers are components shared by IPv4
network stack and IPv6 network stack. These modules could be found in
MdeModulePkg except the UNDI driver. For UNDI driver, please contact the
Card Vendor.

Note: The PXE driver (NetworkPkg/UefiPxeBcDxe) supports boot on both
IPv4 and IPv6 network stack.

Note: Supported features in IPsec

- 1\) Security Protocols Encapsulating Security Payload (ESP)
- 2\) IPsec Mode Transport/Tunnel mode
- 3\) Encryption Algorithm 3DES-CBC, AES-CBC
- 4\) Authentication Algorithm HMAC_SHA1_96
- 5\) Authentication Method Pre-shared Key, X509 Certificates

Note: After IPsec is enabled in both side, all inbound and outbound IP
packet are processed by IPsec.

More information: [NetworkPkg Getting Started
Guide](networkpkg_getting_started_guide.md)
