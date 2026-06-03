# Minimum Platform Architecture

The Minimum Platform Architecture (MPA) creates guidelines for how to design, enable improvements in quality and
security on open source platform initialization development on firmware for Intel products.

#### MinPlatformPkg

The main MinPlatform Package
([MinPlatformPkg](https://github.com/tianocore/edk2-platforms/tree/master/Platform/Intel/MinPlatformPkg)) provides a
consistent boot flow and well-defined interfaces to support board functions.

- How to download and build
  [https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/Readme.md](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/Readme.md)

#### EDK II Minimum Platforms

- [Kaby Lake MinPlatform](../../platforms-packages/platform-ports/kaby_lake_minplatform.md) - EDK II platform firmware
  on 7th Generation Intel® Core™ Processors and chipsets (formerly [Kaby
  Lake](https://ark.intel.com/products/codename/82879/Kaby-Lake) platforms).
- [Project Olympus (Purley MinPlatform)](../../platforms-packages/platform-ports/purley_minplatform.md) - EDK II
  platform firmware for the Open Compute Project
  (OCP) [Intel XSP
  Motherboard](https://www.opencompute.org/wiki/Server/ProjectOlympus#Project_Olympus_Server_Motherboards).

    Note: Purley board support is no longer available in the codebase.
- [Whiskey Lake MinPlatform](../../platforms-packages/platform-ports/whiskey_lake_minplatform.md) - EDK II platform
  firmware on 8th Generation Intel® Core™ Processors and chipsets (formerly [Whiskey
  Lake](https://ark.intel.com/content/www/us/en/ark/products/codename/135883/whiskey-lake.html) platforms).

#### Resources

- [Open Source Firmware Conference (OSFC)](https://www.osfc.io/) Presentation on [Minimum Platform Architecture (Sept
  2019)](https://software.intel.com/sites/default/files/Open%20Source%20UEFI%20Firmware%20for%20Intel%20Platforms.pdf)
- [EDK II Minimum Platform Architecture
  Specification](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/)
- [A Tour Beyond BIOS Open Source IA Firmware Platform Design Guide in EDK II (v
  2)](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/MinPlatformPkg/Docs/A_Tour_Beyond_BIOS_Open_Source_IA_Firmware_Platform_Design_Guide_in_EFI_Developer_Kit_II%20-%20V2.pdf)
