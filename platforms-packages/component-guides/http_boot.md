# UEFI HTTP Boot

For a detailed description on UEFI HTTP Boot, see the "HTTP Boot" section of the [UEFI
Specification](https://www.uefi.org/specifications).

## HTTP Boot: Getting Started

Please refer to the white paper [EDK II HTTP Boot Getting Started
Guide](https://tianocore-docs.github.io/EDKIIHttpBootGettingStartedGuide/draft/) for a step by step guide of the HTTP
Boot enabling and server configuration in **corporate environment**.

## Vendor Documentation and Conference Presentations

### SUSE/openSUSE

* [UEFI HTTPBoot with OVMF](https://en.opensuse.org/UEFI_HTTPBoot_with_OVMF)
* [UEFI HTTPBoot Server Setup](https://en.opensuse.org/UEFI_HTTPBoot_Server_Setup)

### RedHat/Fedora

* [grub-https.cfg](https://gist.github.com/vathpela/e721e0ed7d6ade6a9444e7a94b66a4da)

### Foreman

* [RFC: UEFI HTTP booting](https://community.theforeman.org/t/rfc-uefi-http-booting/8723)

### HPE

* [UEFI Deployment Guide for HPE ProLiant Gen10 Servers and HPE
  Synergy](https://support.hpe.com/hpsc/doc/public/display?docId=emr_na-a00016376en_us)
* [UEFI HTTP/HTTPS Boot, LinuxCon China 2016](https://www.slideshare.net/LCChina/uefi-httphttps-boot)

### Lenovo

* [Using HTTP Boot to Install an Operating System on Lenovo ThinkSystem servers, Nov
  2017](https://lenovopress.com/lp0736.pdf)

## URI Configuration

Besides the standard DHCP parameters like the station IP, gateway and DNS server address, the EDK II HTTP Boot driver
will use the extensions below, assigned by the DHCP server in a corporate environment.

| Tag Name | Tag # (DHCPv4) | Tag # (DHCPv6)| Length | Data Field |
| --- | --- | --- | --- |--- |
| Boot File | 'file' field in DHCP header, or option 67 | 59 | Varies | Boot File URI String (eg. "[https://Webserver/Boot/Boot.efi](https://Webserver/Boot/Boot.efi)" or "[https://Webserver/Boot/Boot.iso](https://Webserver/Boot/Boot.iso)") |
| Class Identifier | 60 | 16 | 10 | "HTTPClient" |

### URI Configuration in Home Environment

Unlike corporate networks, the DHCP server in a typical home network environment is only available for host IP
configuration assignment. This means the boot file URI must be entered by the user instead of the DHCP HTTPBoot
extensions.
![Home Network Topology](../../Projects/NetworkPkg/images/Home.png)

The EDK II HTTP Boot driver provides a configuration page for the boot file URI setup.

1. In the main page of Boot Manager Menu, enter **Device Manager** -> **Network Device List** -> Select a NIC device ->
**HTTP Boot Configuration**, set the HTTP boot parameters such as the boot option title, IP start version and the URI
address as below.
![HTTP Boot Configuration
Page](../../Projects/NetworkPkg/images/URI_Address.PNG)
2. Save the configuration and back to the main page, enter **Boot Manager** menu as below, select the new created boot
option to start the HTTP Boot.
![Boot Manager Menu](../../Projects/NetworkPkg/images/Boot_Option.PNG)
3. To delete the boot option, enter **Boot Maintenance Manager** -> **Boot Options** -> **Delete Boot Option**.

## RAM Disk Boot from HTTP

Besides the UEFI formatted executable image, the downloaded boot file could also be an archive file (hard disk image) or
an ISO image. The file should contain an UEFI-compliant file system and will be mounted as a RAM disk by HTTP Boot
driver, to be proceeded in the subsequent boot process.

In EDK II HTTP Boot driver, the image type is identified by the media type, or the filename extensions if "Content-Type"
header is not present in the HTTP response message.

| Media Type | Filename Extensions | Image Type |
|---| --- | --- |
|[application/vnd.efi.iso](https://www.iana.org/assignments/media-types/application/vnd.efi-iso)|*.iso|Virtual CD Image|
|[application/vnd.efi.img](https://www.iana.org/assignments/media-types/application/vnd.efi-img)|*.img|Virtual Disk Image|
|[application/efi](https://www.iana.org/assignments/media-types/application/efi)|Others (typically *.efi)|UEFI Executable Image|

### RAM Disk Image Size

According to section 5.2.25.2 ("System Physical Address (SPA) Range Structure") of the ACPI 6.2 specification, UEFI
firmware will try to allocate memory from **Reserved** memory range to store the downloaded boot image. The maximum RAM
disk image size depends on how much continuous reserved memory block the platform could provide.
> System physical address ranges described as address type Virtual CD or Virtual Disk shall be described as EFI
Reserved Memory Type in UEFI GetMemoryMap API (AddressRangeReserved in E820 Table).

## Feature Enabling

UEFI HTTP/HTTPS Boot is supported in UDK2017 and subsequent release.

Prior to these releases, code patches are required. Besides the HttpDxe/HttpBootDxe driver, the RamDiskDxe driver and
the UefiBootManagerLib ([commit
b1bb6f5](https://github.com/tianocore/edk2/commit/b1bb6f5961d82f30046e39e187a80556250f2bd1), [commit
fb5848c](https://github.com/tianocore/edk2/commit/fb5848c588688d1e3cd3f175ff888549adddd024) and [commit
3a986a3](https://github.com/tianocore/edk2/commit/3a986a353db249e3ae128d47bff3a13c6e13a037)) are also required.

```
[LibraryClasses]
  UefiBootManagerLib|MdeModulePkg/Library/UefiBootManagerLib/UefiBootManagerLib.inf
[Components]
  MdeModulePkg/Universal/Disk/RamDiskDxe/RamDiskDxe.inf
```

UEFI HTTPS Boot is preferred over HTTP due to security considerations (only HTTPS is allowed by default). Please modify
PCD settings to **TRUE** to enable the feature.

```
  gEfiNetworkPkgTokenSpaceGuid.PcdAllowHttpConnections
```
