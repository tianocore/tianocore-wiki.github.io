# Host-Based Firmware Analyzer

Host-based Firmware Analyzer (HBFA) enables advanced testing of UEFI drivers and UEFI Platform Initialization (PI)
drivers in the developer's OS environment. This test system was contributed to TianoCore edk2-staging branch by Intel in
April 2019.

[https://github.com/tianocore/edk2-staging/tree/HBFA](https://github.com/tianocore/edk2-staging/tree/HBFA)

## Background

Computer platform firmware is a critical element in the root-of-trust. Firmware developers need a robust tool set to
analyze and test firmware components, enabling detection of security issues prior to platform integration and helping to
reduce validation costs. HBFA allows developers to run open source advanced tools, such as fuzz testing, symbolic
execution, and address sanitizers in a system environment.

## Additional Information

* [Using Host-based Analysis to Improve Firmware
  Resiliency](https://www.intel.com/content/dam/develop/external/us/en/documents/intel-usinghbfatoimproveplatformresiliency-820238.pdf)
  (whitepaper)
