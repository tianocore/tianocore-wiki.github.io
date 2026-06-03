# Obsolete Tasks

**:red_circle: It is not recommended that anyone work on these tasks.
:red_circle:**

These are tasks had been previously identified by our community but for
various reasons are no longer relevant and are largely historical. We do
not plan to accept any GSoC proposals for these projects. Please let us
know on [https://edk2.groups.io/g/devel](https://edk2.groups.io/g/devel)
if you plan to work on one of these tasks anyway (to prevent duplicated
effort).#

## Build a C compiler for EBC

**:red_circle: Low priority because EBC is no longer required by the
UEFI spec. :red_circle:**

Build a C compiler that is able to generate EBC (EFI Byte Code).

- Difficulty: Hard
- Language: C, Assembly
- Mentor:
- Suggested by: [https://github.com/nate-desimone](https://github.com/nate-desimone)

More information:
[Tasks-Build-a-C-compiler-for-EBC](../../governance/project-management/tasks_build_a_c_compiler_for_ebc.md)

## Compatibility Support Module

**:red_circle: Low priority because TianoCore does not promote legacy
boot architectures. :red_circle:**

A [CSM](../../reference/specs-standards/compatibility_support_module.md) allows a UEFI
system to boot legacy operating systems on IA32 & X64 systems. In other
words, most current production OS's. Strong preference is for a BSD
licensed solution, as this is license used by nearly all of our code. A
SeaBIOS based solution could be considered, but would not be as widely
usable (GPL license). [https://www.coreboot.org/SeaBIOS](https://www.coreboot.org/SeaBIOS)

- Difficulty: Hard
- Language: Assembly, C
- Mentor:
- Suggested by: bjjohnson
- Status: No longer a priority, since UEFI OS support is mainstream.
  SeaBIOS can be built as a CSM, but a BSD licensed alternative is still
  not available.

## Intel Galileo Platform Improvements

**:red_circle: Low priority because Galileo is no longer manufactured.
:red_circle:**

Add features to
[Galileo](../../platforms-packages/platform-ports/galileo.md)
based on open source platform in EDK II.
[https://github.com/tianocore/edk2/tree/master/QuarkPlatformPkg](https://github.com/tianocore/edk2/tree/master/QuarkPlatformPkg)

- Switch from private SD/MMC stack to standard one in
  [MdeModulePkg](../../platforms-packages/core-packages/mde_module_pkg.md).
- Debugger/Console using USB client mode for multiple high perf UARTs in
  ESRAM
- Logging and Debugger support over USB using ESRAM for DMA.

Mentor: [https://github.com/mdkinney](https://github.com/mdkinney)

## Coverity Static Analysis Tools

**:red_circle: Low priority for now because one-off Coverity scans have
been done and those bugs still need to be fixed. :red_circle:**

Enable Coverity Scan for all EDK II projects.
[https://scan.coverity.com/](https://scan.coverity.com/)

- Evaluate open source CI tools to fine best one to periodically auto
  run Coverity static analysis
- Look into ways to auto-post results to TianoCore (website, wiki, or
  git repo).
- Look into ways to automate GitHub issue generation based on Coverity
  results.

Mentor: [https://github.com/mdkinney](https://github.com/mdkinney),
[https://github.com/shijunjing](https://github.com/shijunjing)

## See Also

- [How To Contribute](../../development/contribution-guides/how_to_contribute.md)
