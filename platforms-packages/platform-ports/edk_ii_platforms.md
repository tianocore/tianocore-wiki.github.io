# EDK II Platforms

## EDK II Platforms

Note: new platforms are being developed in the [edk2-platforms](https://github.com/tianocore/edk2-platforms) repository.
Some older platforms still reside in the main [edk2](https://github.com/tianocore/edk2) repository.

### Virtual/Simulated Platforms

* [OVMF](ovmf.md) - UEFI firmware support for the [QEMU](https://www.qemu.org/) open source machine emulator and
  virtualizer. OVMF supports x86\_64 while there are LoongArchVirt and RiscVVirt platforms which support running
  on QEMU LoongArch and RISC-V platforms.
* [bhyve](bhyve.md) - UEFI firmware for the [bhyve](https://bhyve.org) hypervisor on the BSDs (FreeBSD/Illumos bhyve,
  FreeBSD/NetBSD Xen, OpenBSD vmm, NetBSD Xen/nvmm/HAXM etc.).
* [EmulatorPkg](emulator_pkg.md) - enable UEFI emulation within an OS environment.
* [ArmVirtPkg](../core-packages/arm_virt_pkg.md) - UEFI emulation for ARM processors.

### Intel® Processor Platforms

Recent Intel platform EDK II implementations follow a software architecture intended to aid in uniform delivery of Intel
platforms called EDK II Minimum Platform. That architecture is described and maintained in the [EDK II Minimum Platform
Specification draft](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/). Brief and practical
information regarding the goals of a Minimum Platform and how to build are available in the Intel platform
[Readme.md](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/Readme.md).

#### EDK II Minimum Platforms

* [Alder Lake MinPlatform](alder_lake_minplatform.md) - EDK II platform firmware on 12th Generation Intel® Core™
  Processors and chipsets (formerly [Alder Lake]
(https://www.intel.com/content/www/us/en/ark/products/codename/147470/products-formerly-alder-lake.html) platforms).
  It should be possible to run EDK II firmware on the AAEON UP Squared i12 board.

### 64-bit ARM (AArch64 - ARMv8-A and ARMv9-A) Processor Platforms

* [ADLINK Ampere Altra Dev Kit]()
* [ASRock Rack ALTRAD8UD-1L2T]()
* [Ampere Mt. Jade]()
* [ARM Morello]()
* [ARM N1 SDP]()
* [ARM RD N2 FVP]()
* [ARM RD V3]()
* [ARM RD V3 R1]()
