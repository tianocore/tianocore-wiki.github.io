# [UDK2018](udk2018.md) key features and changes

* Industry Standards & Public Specifications
  * [UEFI 2.7](https://www.uefi.org/sites/default/files/resources/UEFI_Spec_2_7.pdf )
    * Deprecate SMM Communication ACPI Table
    * Update Reset Notification Protocol
    * Add HII Popup Protocol
    * Add UEFI UFS DEVICECONFIG Protocol
    * Add Partition Information Protocol
    * Simplify Secure Boot Revocation & usage of VerifySignature
    * Add DNS device path
    * Add EFI HTTP Boot Callback Protocol
    * Add new data type to EFI Supplicant Protocol
    * Allow SetData to clear configuration in Ip4Config2/Ip6Config
  * [UEFI PI 1.6](https://www.uefi.org/sites/default/files/resources/PI_Spec_1_6.pdf ) & [PI
    1.5](../../reference/specs-standards/pi.md)
    * Allow SEC to pass HOBs into PEI
    * Handle PEI PPI descriptor notifications from SEC
    * Add Pre-permanent memory page allocation
    * Add FV Extended Header entry containing used FV size
    * Add additional alignment in FFS file
    * Support standalone MM module generation
  * [ACPI 6.2](https://www.uefi.org/sites/default/files/resources/ACPI_6_2.pdf )
    * Add ACPI IO Remapping Table (IORT) definition
* IOMMU-based DMA Protection (in [IntelSiliconPkg]( https://github.com/tianocore/edk2/tree/UDK2018/IntelSiliconPkg) )
* Support Stack Guard, Heap Guard and NULL Pointer Detection
* Update OpenSSL version to the new 1.1.0g.
* Support CPU PPIN, LMCE and PROC_TRACE feature.
* Add OpalPassword solution without SMM device code.
* Update performance infrastructure to save perf entry in ACPI FPDT table.
* Remove TrEE libraries and drivers.
* Support Structure PCD as Centralized Configuration Management.
* Add Microsoft Visual Studio 2017 tool chain in BaseTools tools_def.template.
* Support hash-based build to improve the incremental build performance
* Build time improvement using multi-threading in GenFds to generate FFS files
* Support XCODE5 tool chain build and boot functionality.

**********
**Note:** This page describes the  package notes and the differences based on previous UEFI Development Kit
([UDK](udk.md)) [UDK2017](udk2017.md) Release.

* For a detailed list of Changes and updates,  See wiki page [UDK2018](udk2018.md) Release

*******---
