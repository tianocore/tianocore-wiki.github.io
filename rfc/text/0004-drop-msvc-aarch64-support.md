# RFC: Drop MSVC AARCH64 Support

## Metadata

- **RFC Number**: 0004
- **Title**: Drop MSVC AARCH64 Support
- **Status**: Accepted

## Change Log

- 2026-07-13: Initial RFC created

## Motivation

edk2 has build bindings for MSVC AARCH64 but lacks support for this toolchain in most
of the repository. The toolchain is not exercised in CI and a full platform cannot be built with
it without significant downstream overrides. One large blocker is that GAS assembly is not supported
by the toolchain, but the majority of the AARCH64 assembly code in edk2 is GAS only.

The benefit of MSVC AARCH64 has been that it natively produces PE/COFF binaries (and so thereby allowing
different file and section alignment, PDB debugging, and less processing). However, the newly added
CLANGPDB AARCH64 also natively produces PE/COFF binaries, is supported on Linux and Windows, uses
GAS assembly, and is run in CI today.

Pseudo supporting additional toolchains carries maintenance burden and risk, especially as this
toolchain will never be tested. Consumers should move to CLANGPDB AARCH64 instead and MSVC
AARCH64 should be removed.

## Technology Background

CLANGPDB AARCH64 support was added to edk2 in https://github.com/tianocore/edk2/pull/11902 in
early 2026. Prior to this, MSVC AARCH64 was the only toolchain with edk2 bindings for AARCH64 that
supported PE/COFF binary generation natively. However, all of the build support was primarily
downstream and toolchain adoption never really grew, with GCC AARCH64 being the primary AARCH64
toolchain in edk2.

## Goals

1. Reduce toolchain maintenance
2. Remove dead/almost dead/forever untested code

## Requirements

1. Have a better alternative for consumers to migrate to

## UEFI/PI Specification Impact

This does not have specification impact.

## Backward Compatibility

This is a breaking change. edk2 will not support building with MSVC AARCH64 any longer. However,
as noted, a full platform cannot be built with MSVC AARCH64 with edk2 anyway.

Consumers are expected to move to CLANGPDB AARCH64, which is a better alternative for the reasons
listed above. GCC AARCH64 also exists as a possibility, but does not run on Windows, where MSVC
AARCH64 developers would be.

## Platform/Package Impact

- All packages that support AARCH64 that can be built with MSVC today.
- All AARCH64 platforms using MSVC AARCH64 (not believed to exist without heavy downstream overrides).

## Unresolved Questions

N/A.

## Prior Art/Related Work

Toolchains have been deprecated in edk2 before, but primarily older versions rather than
an entire toolchain.

## Alternatives

### Alternative 1: Do Nothing

- **Pros**: No breaking change.
- **Cons**: Unused code is kept in the tree, there is the appearance of support for what will likely
            be a painful experience for consumers.
- **Why not chosen**: Consumers will be better served by moving to the actually supported CLANGPDB AARCH64.
                      edk2 maintenance will be improved by removing unused code.

## Implementation Design

- VS2019, VS2022, and VS2026 will drop AARCH64 bindings. Future VS toolchains will not include them.
- Supporting arm64asm files will be removed (anything `/**/AArch64/*.asm`).
- All `#if defined _(MSC_VER)` will be removed from AARCH64 code.
- All `MSFT` toolchain specifiers in AARCH64 INFs will be removed along with any associated files.

### Architecture Overview

N/A.

### Detailed Design

N/A.

### Code Examples

N/A.

## Testing Strategy

N/A.

## Migration/Adoption Plan

1. **Phase 1**: Announce deprecation of the toolchain
2. **Phase 2**: Remove the toolchain

## Guide-Level Explanation

### For Package Developers

MSVC AARCH64 is no longer supported. Migrate to CLANGPDB AARCH64 instead.

### For Platform Developers

MSVC AARCH64 is already unsupported for platform developers, but the
same guidance applies.

### For End Users (if applicable)

N/A.
