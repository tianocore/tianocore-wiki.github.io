# EDK II Security White Papers

A list of White Papers and information for EDK II Security from multiple sources

* [https://uefi.org](https://uefi.org)
* [https://software.intel.com/en-us/firmware/](https://software.intel.com/en-us/firmware/)
* [https://www.tianocore.org](https://www.tianocore.org)
* Industry standard:
  * NIST: [https://csrc.nist.gov/publications/sp800](https://csrc.nist.gov/publications/sp800)
  * TCG: [https://trusted.computinggroup.com/](https://trusted.computinggroup.com/)
* SideChannel: [Intel Software Developer Zone -firmware speculative
  execution](https://www.intel.com/content/www/us/en/developer/articles/technical/software-security-guidance/technical-documentation/host-firmware-speculative-side-channel-mitigation.html)
* MDS: [Intel Software Developer Zone - microarchitectural data
  sampling](https://www.intel.com/content/www/us/en/developer/articles/technical/software-security-guidance/technical-documentation/intel-analysis-microarchitectural-data-sampling.html)

**General:**

* [Book - building secure firmware](https://www.amazon.com/Building-Secure-Firmware-Armoring-Foundation/dp/1484261054)
  (October 2020)
* [uefi.org - An Introduction to Platform
  Security](https://www.uefi.org/sites/default/files/resources/Intel_An%20Introduction%20to%20Platform%20.pdf) (Spring
  2018)
* [uefi.org - Threat Modeling for Modern System
  FW.pdf](https://www.uefi.org/sites/default/files/resources/Intel-UEFI-ThreatModel.pdf) (July 2013)

**EDK II Code:**

* [A Tour Beyond BIOS - Security Design Guide
  in_EDK_II.pdf](https://github.com/tianocore-docs/Docs/blob/main/White_Papers/A_Tour_Beyond_BIOS_Security_Design_Guide_in_EDK_II.pdf)
  (Sept 2016)
* [EDK II Secure Coding Guide](https://tianocore-docs.github.io/EDK_II_Secure_Coding_Guide/draft/) (June 2019)
* [EDK II Secure Code Review Guide](https://tianocore-docs.github.io/EDK_II_Secure_Code_Review_Guide/draft/) (June 2019)
* [OCP - Secure Firmware Development Best
  Practices](https://github.com/opencomputeproject/Security/blob/master/SecureFirmwareDevelopmentBestPractices.md) (May
  2020)
* [Universal Scalable Firmware - Security](https://universalscalablefirmware.github.io/documentation/5_security.html)
  (October 2021)

**Memory Protection:**

* [A Tour Beyond BIOS – Memory Protection in UEFI BIOS -
  gitbook](https://tianocore-docs.github.io/ATBB-Memory_Protection_in_UEFI_BIOS/draft/) (March 2017)
* [A Tour Beyond BIOS - Mitigate Buffer Overflow in
  UEFI](https://tianocore-docs.github.io/ATBB-Mitigate_Buffer_Overflow_in_UEFI/draft/) (April 2018)

**SMM Protection:**

* [A Tour Beyond BIOS Secure SMM
  Communication](https://github.com/tianocore-docs/Docs/blob/main/White_Papers/A_Tour_Beyond_BIOS_Secure_SMM_Communication.pdf)
  (April 2016)
* [uefi.org - SMM Protection in EDK
  II](https://www.uefi.org/sites/default/files/resources/Jiewen%20Yao%20-%20SMM%20Protection%20in%20%20EDKII_Intel.pdf)
  (Spring 2017)

**SecureBoot/AuthVariable:**

* [Understanding the UEFI Secure Boot
  Chain](https://tianocore-docs.github.io/Understanding_UEFI_Secure_Boot_Chain/draft/) (June 2019)
* [A Tour Beyond BIOS - Implementing UEFI Authenticated Variables in SMM with EDK
  II](https://github.com/tianocore-docs/Docs/blob/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_UEFI_Authenticated_Variables_in_SMM_with_EDKII_V2.pdf)
  (Oct 2015)

**TrustedBoot/TPM2:**

* [Understanding the Trusted Boot Chain
  Implementation](https://tianocore-docs.github.io/edk2-TrustedBootChain/release-1.00/edk2-TrustedBootChain-release-1.00.pdf)
  (Nov 2020)
* [A Tour Beyond BIOS - with the UEFI TPM2 Support in EDK
  II](https://www.intel.com/content/www/us/en/content-details/671464/a-tour-beyond-bios-with-the-uefi-tpm2-support-in-edk-ii.html)
  (Sept 2014)
* [FSP2 Measurement and Attestation](https://cdrdv2.intel.com/v1/dl/getContent/644001) (July 2021)
* [uefi.org - Traceable Firmware Bill of Materials
  Overview](https://uefi.org/sites/default/files/resources/Traceable%20Firmware%20Bill%20of%20Materials%20-%2020211207%20-%20007.pdf)

**DMA:**
[A Tour Beyond BIOS - Using IOMMU for DMA Protection in UEFI firmware](https://www.intel.com/content/dam/develop/external/us/en/documents/intel-whitepaper-using-iommu-for-dma-protection-in-uefi-820238.pdf)
(Oct 2017)

**Capsule/Recovery:**
[A Tour Beyond BIOS - Capsule Update and Recovery in EDK II](https://github.com/tianocore-docs/Docs/blob/main/White_Papers/A_Tour_Beyond_BIOS_Capsule_Update_and_Recovery_in_EDK_II.pdf)
(Dec 2016)

**S3:**
[A Tour Beyond BIOS - Implementing S3 Resume with EDK II](https://github.com/tianocore-docs/Docs/blob/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_S3_resume_with_EDKII_V2.pdf)
(Oct 2015)

**Profile:**
[A Tour Beyond BIOS - Implementing Profiling in EDK_II](https://github.com/tianocore-docs/Docs/blob/main/White_Papers/A_Tour_Beyond_BIOS_Implementing_Profiling_in_EDK_II.pdf)
(July 2016)

**STM/VMM:**

* [A Tour Beyond BIOS - Launching STM to Monitor SMM in EDK
  II](https://www.intel.com/content/dam/develop/external/us/en/documents/a-tour-beyond-bios-launching-stm-to-monitor-smm-in-efi-developer-kit-ii-819978.pdf)
  (Aug 2015)
* [A Tour Beyond BIOS - Launching a VMM in EDK
  II](https://www.intel.com/content/dam/develop/external/us/en/documents/a-tour-beyond-bios-launching-vmm-in-efi-developer-kit-ii-0-819978.pdf)
  (Oct 2015)
* [A Tour Beyond BIOS - Supporting SMM Resource Monitor using EDK
  II](https://www.intel.com/content/dam/develop/external/us/en/documents/a-tour-beyond-bios-launching-stm-to-monitor-smm-in-efi-developer-kit-ii-819978.pdf)
  (June 2015)

**StandaloneMM:**
[A Tour Beyond BIOS - Launching Standalone SMM Drivers in the PEI Phase using EDK
II](https://www.intel.com/content/dam/develop/public/us/en/documents/a-tour-beyond-bios-launching-standalone-smm-drivers-in-pei-using-the-efi-developer-kit-ii.pdf)
(May 2015)
