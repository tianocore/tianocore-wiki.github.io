# Capsule Based Firmware Update And Firmware Recovery

[Back to EDK II Features](../../platforms-packages/core-packages/edkii_packages.md)

This page describes the UEFI Capsule ("capsule") implementation in EDK II, along with common use cases. [EDK
II](https://github.com/tianocore/edk2) provides an implementation of capsule-based firmware update and firmware recovery
features that can detect if a firmware update or a recovery image delivered via UEFI Capsule has been modified
([`SignedCapsulePkg`](https://github.com/tianocore/edk2/tree/master/SignedCapsulePkg)). It can also verify that the
capsule applies to the platform that receives the capsule, and verifies that a firmware update does not violate any of
the platforms's firmware rollback rules.

## Why Are Signed Capsules Important?

Firmware is responsible for low-level platform initialization and hand-off to the operating system. This means firmware
establishes root-of-trust for the platform. Signed capsules help assure that the correct update is being applied to the
platform. Using signed images with UEFI Capsule allows an OS-agnostic process to provide verified firmware updates,
utilizing root-of-trust established by the firmware. This scenario assumes the factory-provisioned firmware and
subsequent updates are signed with the same public/private keypair.

## UEFI Capsule Implementation in EDK II

[`SignedCapsulePkg`](https://github.com/tianocore/edk2/tree/master/SignedCapsulePkg) makes use of
[OpenSSL](https://www.openssl.org/) command line utilities to sign firmware update capsules and firmware recovery
images. It also uses OpenSSL libraries to authenticate firmware update capsules and firmware recovery images before they
are used.

As of December 2016, [`SignedCapsulePkg`](https://github.com/tianocore/edk2/tree/master/SignedCapsulePkg) is integrated
into the master branch of the [Intel® Galileo Gen 2](../../platforms-packages/platform-ports/galileo.md) and [MinnowBoard Max](../../platforms-packages/platform-ports/minnow_board_max.md) platform firmware
projects on EDK II. To implement this package on other EDK II platforms, please review documentation for capsule use
cases.

## Common Use Cases for UEFI Capsule

* [Using a Signed Capsule to Perform a System Firmware Update](../../development/tutorials-howto/capsule_based_system_firmware_update.md)
* [Using a Signed Capsule to Perform a Device Firmware Update](capsule_based_device_firmware_update.md)
* [Using a Capsule to perform a CPU Microcode Update](capsule_based_cpu_microcode_update.md)
* [Using a Signed Capsule as a System Firmware Recovery Image](capsule_based_system_recovery_image.md)

## Signing Keys for Test and Production

The EDK II implementation of capsule-based firmware update and firmware recovery provides **test signing keys** that may
be used during firmware development and debug. If the EDK II implementation of capsule-based firmware update and
recovery is used to build a production firmware images, production firmware updates, or production recovery images, then
the product owner must create and manage their **production signing keys**.

```admonish warning "Key Security Responsibility"
These instructions only cover how to generate a new X.509 Certificate Chain using OpenSSL. It is up to the
product owner to properly handle and protect the cryptographic pair (private keys and public X.509 certificates) used
to sign and authenticate capsule-based system firmware update images.
```

## Summary of UEFI Capsule Services, Tables & Variables

The [UEFI Specification](https://www.uefi.org) and the [PI Specification](https://www.uefi.org) provide details on the
services, tables, and variables associated with the use of capsules for firmware update and recovery.

* \[UEFI\] `UpdateCapsule()` and `QueryCapsuleCapabilities()` Runtime Services
* \[UEFI\] `_OsIndicationsSupported_` and `_OsIndications_` UEFI Variables
  * Support for `EFI_OS_INDICATIONS_FILE_CAPSULE_DELIVERY_SUPPORTED` is **not** implemented.
* \[UEFI\] _CapsuleNNNN_ Capsule Report Variables
* \[UEFI\] Firmware Management Protocol (FMP)
* \[UEFI\] EFI System Resource Table (ESRT)
* \[PI\] Recovery Module PPI
* \[PI\] Device Recovery Module PPI
* \[PI\] Recovery Block I/O PPI and Recovery Block I/O 2 PPI
* \[PI\] Boot to Recovery Mode PPI

## Additional Documentation

[A Tour Beyond BIOS: Capsule Update and Recovery in EDK II](https://github.com/tianocore-docs/Docs/raw/master/White_Papers/A_Tour_Beyond_BIOS_Capsule_Update_and_Recovery_in_EDK_II.pdf)
(Intel Whitepaper, Dec 2016)

[Signed UEFI Firmware Updates in EDK II](https://software.intel.com/en-us/blogs/2017/02/04/signed-uefi-firmware-updates-in-edk-ii)
(Intel Developer Zone)

[Better Firmware Updates in Linux Using UEFI Capsules](https://software.intel.com/en-us/blogs/2015/06/23/better-firmware-updates-in-linux-using-uefi-capsules)
(Intel Developer Zone)

[Back to EDK II Features](../../platforms-packages/core-packages/edkii_packages.md)
