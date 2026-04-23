# Alder Lake MinPlatform

[EDK II Platforms](edk_ii_platforms.md) | [Intel® Processor Platforms](edk_ii_platforms.md#intel-processor-platforms)

***

[AlderlakeOpenBoardPkg](https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/AlderlakeOpenBoardPkg)
is an EDK II for UEFI platform firmware on 12th Generation Intel® Core™ Processors and chipsets (formerly [Alder
Lake](https://www.intel.com/content/www/us/en/ark/products/codename/147470/products-formerly-alder-lake.html) platforms). This specific
package supports the Intel Alder Lake RVP (reference validation platform), it can be used as a simple starting base
to enable support for other boards.

This tree follows a "minimum platform" philosophy, providing boot to a UEFI compliant operating system using the minimum
number of EDK II modules. The project uses the [Intel® Firmware Support Package (Intel®
FSP)](https://github.com/IntelFsp/FSP/tree/master/AlderLakeFspBinPkg) for platform silicon initialization.

[Minimum Platform Readme File](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/Readme.md)
[Minimum Platform Specification](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/)
