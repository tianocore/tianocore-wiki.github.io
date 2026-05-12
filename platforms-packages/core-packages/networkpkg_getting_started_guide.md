# NetworkPkg Getting Started Guide

## Features List

### IPv6 network stack

* **NetworkPkg/Ip6Dxe** - Ip6 driver, which produces

```
EFI_IP6_PROTOCOL
EFI_IP6_CONFIG_PROTOCOL
```

* **NetworkPkg/Udp6Dxe** - Udp6 driver, which produces

```
EFI_UDP6_PROTOCOL
```

* **NetworkPkg/TcpDxe** - TCP combo driver, which produces

```
EFI_TCP4_PROTOCOL
EFI_TCP6_PROTOCOL
```

Note: The Tcp4Dxe driver in MdeModulePkg has been deprecated, please use NetworkPkg/TcpDxe instead.

* **NetworkPkg/Dhcp6Dxe** - DHCP6 driver, which produces

```
EFI_DHCP6_PROTOCOL
```

* **NetworkPkg/Mtftp6Dxe** - MTFTP6 driver, which produces

```
EFI_MTFTP6_PROTOCOL
```

### IPsec

* **NetworkPkg/IpSecDxe** - Ipsec driver, which produces

```
EFI_IPSEC2_PROTOCOL
EFI_IPSEC_CONFIG_PROTOCOL
```

1. Supported features in IPsec:

* Security Protocols: Encapsulating Security Payload (ESP)
* IPsec Mode: Transport/Tunnel mode
* Encryption Algorithm: 3DES-CBC, AES-CBC
* Authentication Algorithm: HMAC\_SHA1\_96
* Authentication Method: Pre-shared Key, X509 Certificates

2. After IPsec is enabled in both side, all inbound and outbound IP packet are processed by IPsec.

### [PXE](../component-guides/pxe.md)

* **NetworkPkg/UefiPxeBcDxe** - PXE driver, which produces

```
EFI_LOAD_FILE_PROTOCOL
EFI_PXE_BASE_CODE_PROTOCOL
```

Note: The UefiPxeBcDxe driver in MdeModulePkg has been deprecated, please use NetworkPkg/UefiPxeBcDxe instead.

### iSCSI

* **NetworkPkg/IScsiDxe** - iSCSi driver, which produces

```
EFI_ISCSI_INITIATOR_NAME_PROTOCOL
EFI_EXT_SCSI_PASS_THRU_PROTOCOL
EFI_AUTHENTICATION_INFO_PROTOCOL
```

Note: The IScsiDxe driver in MdeModulePkg has been deprecated, please use NetworkPkg/IScsiDxe instead.

### DNS

* **NetworkPkg/DnsDxe** - DNS driver, which produces

```
EFI_DNS4_PROTOCOL
EFI_DNS6_PROTOCOL
```

### TLS

* **NetworkPkg/TlsDxe** - TLS driver, which produces

```
EFI_TLS_PROTOCOL
EFI_TLS_CONFIGURATION_PROTOCOL
```

Note: TlsDxe driver takes advantage of OpenSLL library, including BaseCryptLib and TlsLib. So, TLS feature highly
depends on the OpenSSL building. To enable this feature, please follow the instructions found in the file
"Patch-HOWTO.txt" located in CryptoPkg\Library\OpensslLib to enable the OpenSSL building first.

* **NetworkPkg/TlsAuthConfigDxe** - TLS certificates configuration driver, which provides the UI to support the required
  certificate configuration.

### HTTP/HTTPS Boot

* **NetworkPkg/HttpDxe** - HTTP driver, which produces

```
EFI_HTTP_PROTOCOL
```

Note: HttpDxe driver consumes TlsDxe driver to support HTTPS feature. The HTTP instance can be able to determine whether
to use HTTP or HTTPS feature by according to the different schemes ("https://" or "https://") in the boot file URI.

* **NetworkPkg/HttpUtilitiesDxe** - HTTP utilities driver, which produces

```
EFI_HTTP_UTILITIES_PROTOCOL
```

* **NetworkPkg/HttpBootDxe** - HTTP Boot driver, which produces

```
EFI_LOAD_FILE_PROTOCOL
```

### Shell Application

* **Application/VConfig** - VLAN configuration Shell application
* **Application/IpsecConfig** - IPsec configuration Shell application

### Notes

* UNDI, SNP, DPC and MNP drivers are components shared by IPv4 network stack and IPv6 network stack. These modules could
  be found in MdeModulePkg except the UNDI driver. For UNDI driver, please contact the Card Vendor.

## FEATURES ENABLING

### Platform DSC file

```
[Defines]
  DEFINE NETWORK_ENABLE                        = TRUE
  DEFINE NETWORK_IP6_ENABLE                    = TRUE
  DEFINE NETWORK_ISCSI_ENABLE                  = TRUE
  DEFINE NETWORK_VLAN_ENABLE                   = TRUE
  DEFINE NETWORK_IPSEC_ENABLE                  = TRUE
  DEFINE NETWORK_TLS_ENABLE                    = TRUE

[LibraryClasses]
  DpcLib|MdeModulePkg/Library/DxeDpcLib/DxeDpcLib.inf
  NetLib|MdeModulePkg/Library/DxeNetLib/DxeNetLib.inf
  IpIoLib|MdeModulePkg/Library/DxeIpIoLib/DxeIpIoLib.inf
  UdpIoLib|MdeModulePkg/Library/DxeUdpIoLib/DxeUdpIoLib.inf
  TcpIoLib|MdeModulePkg/Library/DxeTcpIoLib/DxeTcpIoLib.inf
  HttpLib|MdeModulePkg/Library/DxeHttpLib/DxeHttpLib.inf
  IntrinsicLib|CryptoPkg/Library/IntrinsicLib/IntrinsicLib.inf
  OpensslLib|CryptoPkg/Library/OpensslLib/OpensslLib.inf
  BaseCryptLib|CryptoPkg/Library/BaseCryptLib/BaseCryptLib.inf
  TlsLib|CryptoPkg/Library/TlsLib/TlsLib.inf

[Components]
  !if $(NETWORK_ENABLE) == TRUE
    MdeModulePkg/Universal/Network/DpcDxe/DpcDxe.inf
    MdeModulePkg/Universal/Network/SnpDxe/SnpDxe.inf
    MdeModulePkg/Universal/Network/MnpDxe/MnpDxe.inf
    MdeModulePkg/Universal/Network/ArpDxe/ArpDxe.inf
    MdeModulePkg/Universal/Network/Dhcp4Dxe/Dhcp4Dxe.inf
    MdeModulePkg/Universal/Network/Ip4Dxe/Ip4Dxe.inf
    MdeModulePkg/Universal/Network/Mtftp4Dxe/Mtftp4Dxe.inf
    MdeModulePkg/Universal/Network/Udp4Dxe/Udp4Dxe.inf
    NetworkPkg/UefiPxeBcDxe/UefiPxeBcDxe.inf
    NetworkPkg/TcpDxe/TcpDxe.inf

    !if $(NETWORK_IP6_ENABLE) == TRUE
      NetworkPkg/Ip6Dxe/Ip6Dxe.inf
      NetworkPkg/Dhcp6Dxe/Dhcp6Dxe.inf
      NetworkPkg/Udp6Dxe/Udp6Dxe.inf
      NetworkPkg/Mtftp6Dxe/Mtftp6Dxe.inf
    !endif

    NetworkPkg/DnsDxe/DnsDxe.inf
    NetworkPkg/HttpDxe/HttpDxe.inf
    NetworkPkg/HttpUtilitiesDxe/HttpUtilitiesDxe.inf
    NetworkPkg/HttpBootDxe/HttpBootDxe.inf

    !if $(NETWORK_IPSEC_ENABLE) == TRUE
      NetworkPkg/IpSecDxe/IpSecDxe.inf
    !endif

    !if $(NETWORK_TLS_ENABLE) == TRUE
      NetworkPkg/TlsDxe/TlsDxe.inf
      NetworkPkg/TlsAuthConfigDxe/TlsAuthConfigDxe.inf
    !endif

    !if $(NETWORK_ISCSI_ENABLE) == TRUE
      NetworkPkg/IScsiDxe/IScsiDxe.inf
    !endif

    !if $(NETWORK_VLAN_ENABLE) == TRUE
      MdeModulePkg/Universal/Network/VlanConfigDxe/VlanConfigDxe.inf
    !endif

  !endif
```

### Platform FDF file

```
!if $(NETWORK_ENABLE) == TRUE
  INF  MdeModulePkg/Universal/Network/SnpDxe/SnpDxe.inf
  INF  MdeModulePkg/Universal/Network/DpcDxe/DpcDxe.inf
  INF  MdeModulePkg/Universal/Network/MnpDxe/MnpDxe.inf
  INF  MdeModulePkg/Universal/Network/ArpDxe/ArpDxe.inf
  INF  MdeModulePkg/Universal/Network/Ip4Dxe/Ip4Dxe.inf
  INF  MdeModulePkg/Universal/Network/Udp4Dxe/Udp4Dxe.inf
  INF  MdeModulePkg/Universal/Network/Dhcp4Dxe/Dhcp4Dxe.inf
  INF  MdeModulePkg/Universal/Network/Mtftp4Dxe/Mtftp4Dxe.inf
  INF  NetworkPkg/TcpDxe/TcpDxe.inf
  INF  NetworkPkg/UefiPxeBcDxe/UefiPxeBcDxe.inf

  !if $(NETWORK_IP6_ENABLE) == TRUE
    INF  NetworkPkg/Ip6Dxe/Ip6Dxe.inf
    INF  NetworkPkg/Dhcp6Dxe/Dhcp6Dxe.inf
    INF  NetworkPkg/Udp6Dxe/Udp6Dxe.inf
    INF  NetworkPkg/Mtftp6Dxe/Mtftp6Dxe.inf
  !endif

  INF  NetworkPkg/DnsDxe/DnsDxe.inf
  INF  NetworkPkg/HttpDxe/HttpDxe.inf
  INF  NetworkPkg/HttpUtilitiesDxe/HttpUtilitiesDxe.inf
  INF  NetworkPkg/HttpBootDxe/HttpBootDxe.inf

  !if $(NETWORK_IPSEC_ENABLE) == TRUE
    INF  NetworkPkg/IpSecDxe/IpSecDxe.inf
  !endif

  !if $(NETWORK_TLS_ENABLE) == TRUE
    INF  NetworkPkg/TlsDxe/TlsDxe.inf
    INF  NetworkPkg/TlsAuthConfigDxe/TlsAuthConfigDxe.inf
  !endif

  !if $(NETWORK_VLAN_ENABLE) == TRUE
    INF  MdeModulePkg/Universal/Network/VlanConfigDxe/VlanConfigDxe.inf
  !endif

  !if $(NETWORK_ISCSI_ENABLE) == TRUE
    INF  NetworkPkg/IScsiDxe/IScsiDxe.inf
  !endif

!endif
```

## Using EDKII Network Stack on NT32

To validate network stack on NT32 platform, please download the source code of
[SnpNt32Io](../component-guides/network_io.md) and refer to the document [UEFI Network
Stack for EDK Getting Started Guide](https://sourceforge.net/projects/network-io/files/Documents/) to build the
SnpNt32Io dynamic library.

## Using EDKII Network Stack on OVMF

To validate network stack on OVMF platform, please refer to the [EDKII Network Stack over
QEMU](../../development/tutorials-howto/edkii_network_over_qemu.md) page.

## UEFI HTTP BOOT

Please refer to the [UEFI HTTP Boot](../component-guides/http_boot.md) page.

## UEFI HTTPS BOOT

Please refer to the [UEFI HTTPS Boot](../component-guides/https_boot.md) page.

## EDKII DPC Protocols

Please refer to the [Defer Procedure Call
Protocol](../../reference/specs-standards/edk_ii_dpc_protocol.md) page.
