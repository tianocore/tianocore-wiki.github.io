# Purley MinPlatform

[EDK II Platforms](edk_ii_platforms.md) | [Intel® Processor Platforms](edk_ii_platforms.md#intel-processor-platforms)

***

## Project Olympus (Purley MinPlatform)

**NOTE:** The Purley MinPlatform code has been removed since maintenance could not be sustained. You may reference the
last commit prior to removal (98c7561279) to retrieve the code.

***

EDK II platform firmware for the Open Compute Project (OCP) [Intel XSP
Motherboard](https://www.opencompute.org/wiki/Server/ProjectOlympus#Project_Olympus_Server_Motherboards). Based on the
[Intel Xeon Scalable Processor](https://ark.intel.com/products/series/125191/Intel-Xeon-Scalable-Processors) family
(formerly codenamed "Purley").

This tree follows a "minimum platform" philosophy, providing boot to a UEFI compliant operating system using the minimum
number of EDK II modules. The project uses binaries in the [edk2-non-osi](https://github.com/tianocore/edk2-non-osi.git)
repository for platform silicon initialization.

Hardware Information:
[https://www.opencompute.org/wiki/Server/ProjectOlympus](https://www.opencompute.org/wiki/Server/ProjectOlympus)
