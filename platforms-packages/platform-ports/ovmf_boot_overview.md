# OVMF Boot Overview

This page provides a high-level overview of the boot process for [OVMF](https://www.tianocore.org/ovmf/).

SEC
===

- UefiCpuPkg/ResetVector/Vtf0
  - Ia16/ResetVectorVtf0.asm: Starts at resetVector
  - Ia16/Real16ToFlat32.asm: Mode Switch: Real =&gt; 32-bit Flat
  - Ia32/SearchForBfvBase.asm: Locates the Boot FirmwareVolume (BFV)
  - Ia32/SearchForSecEntry.asm: Locates the SEC Core with BFV
  - Ia32/Flat32ToFlat64.asm: If 64-bit PEI, then Mode Switch: 32-bit Flat to 64-bit Long
- SEC Core: OvmfPkg/Sec
  - OvmfPkg/Sec/SecMain.c: Finds PEI Core

PEI
===

Pre-EFI Initialization

- PEI Core: MdeModulePkg/Core/Pei
  - Various PEI drivers now run
- DXE IPL (Loader for the DXE Core)
  - Finds DXE Core
  - If PEI is 32-bit and DXE is 64-bit, then Mode Switch: 32-bit Flat to 64-bit Long
  - Jumps to DXE Core

DXE
===

Driver Execution Environment

- DXE Core: MdeModulePkg/Core/Dxe
  - Various DXE/UEFI drivers now run
  - BDS is run after all DXE architectural protocols are available

BDS
===

Boot Device Selection

- BDS: IntelFrameworkModulePkg/Universal/BdsDxe + IntelFrameworkModulePkg/Library/GenericBdsLib
  - OvmfPkg/Library/PlatformBdsLib guides BDS in a platform specific manner
- BDS figures out what to boot, and starts devices to enable the boot
  - BDS may be able to load the EFI shell if it is present in the system firmware file system
