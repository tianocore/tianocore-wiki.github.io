# UDK2017 Key Features and Changes

* Industry Standards & Public Specifications
  * [UEFI 2.6](https://www.uefi.org/sites/default/files/resources/UEFI%20Spec%202_6.pdf)
  * [UEFI PI 1.4a](https://www.uefi.org/sites/default/files/resources/PI_1_4_ErrataA.zip)
  * [UEFI Shell 2.2](https://www.uefi.org/sites/default/files/resources/UEFI_Shell_2_2.pdf)
  * [SMBIOS 3.1.0  3.1.1](https://www.dmtf.org/standards/smbios)
  * [Intel® 64 and IA-32 Architectures Software Developer Manuals](https://software.intel.com/en-us/articles/intel-sdm)
    for model specific register (MSR) definitions
* Storage Technologies
  * [NVMe](https://www.nvmexpress.org/)
* RAM Disk ([UEFI 2.6](https://www.uefi.org/sites/default/files/resources/UEFI%20Spec%202_6.pdf), Section 12.17, RAM Disk
Protocol)
* Compilers
  * [GCC 5.x](https://gcc.gnu.org/gcc-5/)
  * [CLANG/LLVM](https://clang.llvm.org/)
  * [NASM](../../build-tooling/environment-setup/nasm_setup.md)
* [OpenSSL 1.1.0e](https://www.openssl.org/)
* [UEFI HTTP/HTTPS Boot](../../platforms-packages/component-guides/https_boot.md) with TLS
* Adapter Information Protocol
* Regular Expression Protocol
* [Signed Capsule
  Update](../../reference/specs-standards/capsule_based_firmware_update_and_firmware_recovery.md)
* [Signed Recovery
  Images](../../reference/specs-standards/capsule_based_firmware_update_and_firmware_recovery.md)
* SMM Communication Buffer Protections
* SMI Handler Profile
* SMM Page Protection Feature
* STM Launch
* Memory Allocation/Free Profiler
* NX Page Protection in DXE
* [LZMA Compression 16.04](https://7-zip.org/sdk.html)
* [Brotli Compression](https://github.com/google/brotli)
* MP Init Library & CPU Feature Initialization
* EFI Byte Code (EBC) Debugger
* Made HII configuration settings available to OS runtime in memory.
* Add RAM disk driver.
* New packages: IntelFsp2Pkg, IntelFsp2WrapperPkg, IntelSiliconPkg, SignedCapsulePkg

**********
**Note:** This page describes the  package notes and the differences based on previous UEFI Development Kit
([UDK](udk.md)) UDK2015 Release.

* For a detailed list of Changes and updates,  See wiki page [UDK2017](udk2017.md) Release

*******---
