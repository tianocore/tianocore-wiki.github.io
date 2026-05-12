# Tasks UefiCpuPkg CpuDxe MP Support

Enable multiprocessor support for IA32 & X64 within UefiCpuPkg/CpuDxe.
(Implement
[https://github.com/tianocore/edk2/blob/master/MdePkg/Include/Protocol/MpService.h](https://github.com/tianocore/edk2/blob/master/MdePkg/Include/Protocol/MpService.h))

- Status: Complete :heavy_check_mark:
- Difficulty: Medium
- Language: Assembly, C
- Mentor:
- Suggested by: [https://github.com/jljusten](https://github.com/jljusten)

## Details

## Documentation

Intel® 64 and IA-32 Architectures Software Developer's Manual Volume 3A:
System Programming Guide, Part 1

[https://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html](https://www.intel.com/content/www/us/en/processors/architectures-software-developer-manuals.html)

## Development environment

Building: This project can be completed on any edk2 supported OS or
toolchain.

Any assembly code should be ported to all assembly formats.

## Test environments

- OVMF: With QEMU you can use the -smp parameter to enable multiple
  processors
- DUET: With DUET you can run on a system with multiple
  threads/cores/processors

## Sub-goals

Some possible sub-goals for the driver

- Successfully start up other processor (Startup IPI)
- ~~Transition from 16-bit real to 32-bit flat mode~~ (see
  ap-startup-example below)
- ~~Transition from 16-bit real to 64-bit long mode~~ (see
  ap-startup-example below)
- ~~AP starts running CpuDxe code~~ (see ap-startup-example below)
- AP can run code requested by MpService protocol
  (StartupAllAPs/StartupThisAP)
  - See StartCore below
- Support for remaining MpService protocol functions
- ~~Dynamically update ACPI tables in OVMF based on number of
  processors~~
- Refer to "Software Developer's Manual" to research further
  initialization requirements.

## Getting Started

- Code to start from:
  [https://github.com/jljusten/edk2/tree/ap-startup-example](https://github.com/jljusten/edk2/tree/ap-startup-example)
  - This code already starts the APs. This should provide most assembly
    code needed for this task.
  - Tested with QEMU/OVMF on Linux
- The StartCore project provides a sample program that uses the
  MpServices protocol
    [https://svn.code.sf.net/p/edk2-startcore/code/StartCorePkg](https://svn.code.sf.net/p/edk2-startcore/code/StartCorePkg)
    (svn)
- [EmulatorPkg](../../platforms-packages/platform-ports/emulator_pkg.md) already supports MpServices, so
  this may be used as a reference.

## Further Discussion

This project would be for an edk2 based driver, so please discuss the
project on [https://edk2.groups.io/g/devel](https://edk2.groups.io/g/devel).
For IRC, \#edk2 on [irc.oftc.net](https://www.oftc.net).

## See Also

- [Tasks#Multiprocessor
  support in UefiCpuPkg/CpuDxe](tasks_ueficpupkg_cpudxe_mp_support.md)
