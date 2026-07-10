# RFC: Introduce SecureZeroMemoryLib for Securely Erasing Sensitive Buffers

## Metadata

- **RFC Number**: TBD
- **Title**: Introduce SecureZeroMemoryLib for Securely Erasing Sensitive Buffers
- **Status**: Draft
- **Author**: Kun Qin (`@kuqin12`)

## Change Log

- 2026-05-27: Initial RFC created. Motivated by discussion on
  [edk2 PR #12244](https://github.com/tianocore/edk2/pull/12244), with review
  input from `@mdkinney`, `@mikebeaton`, and `@jaykrell`.
- 2026-07-10: Pivoted from an elision-based argument to an ordering issue. Also
  added the "`volatile` is not a barrier" demo.

## Motivation

EDK II ships two large bodies of security-sensitive code that both
explicitly call `SecureZeroMemory` on their secret material:

- [`CryptoPkg`](https://github.com/tianocore/edk2/tree/master/CryptoPkg)
  is a port of OpenSSL, which calls `SecureZeroMemory` (and
  `OPENSSL_cleanse`) on private keys, key schedules, IVs, and
  intermediate cryptographic state.
- [`TcgTpmPkg`](https://github.com/tianocore/edk2/tree/master/TcgTpmPkg)
  is a port of the Microsoft TPM 2.0 reference implementation, which
  calls `SecureZeroMemory` on TPM session secrets, authorization values,
  and HMAC state.

Neither codebase implements `SecureZeroMemory` itself; both inherit it
from their host C runtime. The compatibility shim that supplies it under
EDK II (in their respective CrtLibSupport.h files) currently provides:

```c
#ifndef SecureZeroMemory
#define SecureZeroMemory(ptr, sz)  memset((ptr), 0, (sz))
#endif
```

Combined with the earlier `#define memset(dest, ch, count)  SetMem(...)`
in the same headers, every `SecureZeroMemory(p, n)` call written by
upstream OpenSSL or the TCG TPM reference stack expands, in EDK II
builds, to a plain `SetMem(p, n, 0)`, which is the same as a regular `ZeroMem()`.
The secure-erase intent the upstream authors wrote into their code does not
survive the compiler reordering optimization.

In firmware, the consequences could cause the secret is still zeroed, but only *after*
the buffer has been advertised as clean, so an asynchronous observer (another core,
a DMA engine, an SMI/SMM handler, an allocator reissuing the block) can read live
key material in the interim. Firmware has no process teardown to fall back on.

## Technology Background

Dead-store elimination (DSE) is a standard, decades-old compiler optimization
that removes stores whose values are never observed. It is correct under the
C abstract machine. ISO/IEC 9899 Section 5.1.2.3 - 4: *Program execution* states
(publicly accessible as the C11 final committee draft
[N1570](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n1570.pdf); the
text is materially unchanged through C17 and C23):

```text
An actual implementation need not evaluate part of an expression if it can deduce that its value is not used and that no needed side effects are produced (including any caused by calling a function or accessing a volatile object).
```

A buffer that is wiped and then never read again satisfies exactly that
test: Once the call to the clearing routine has been inlined and its `volatile`
qualifier no longer binds the call site. The compiler may conclude that no needed
side effects are produced. When the buffer in question held a secret, this is
precisely what produces CWE-14.

The security and compiler communities have converged on the answer for the
last twenty-plus years: provide a *separately-named* memory-clearing API
whose contract is explicitly to defeat the optimizer. Every major platform
and security project ships one:

| Project / standard | Optimization-resistant API | General-purpose API |
| --- | --- | --- |
| Win32 / Microsoft | `SecureZeroMemory` | `ZeroMemory` / `RtlZeroMemory` |
| OpenBSD / glibc / FreeBSD | `explicit_bzero(3)` | `bzero` / `memset` |
| Linux kernel | `memzero_explicit()` | `memset()` |
| ISO C11 Annex K | `memset_s` | `memset` |
| OpenSSL / BoringSSL | `OPENSSL_cleanse` | `memset` |
| mbedTLS | `mbedtls_platform_zeroize` | `memset` |
| libsodium | `sodium_memzero` | `memset` |
| NSS | `PORT_Memset` + explicit volatile wrappers | `memset` |

This table presents a *convergent design under independent constraints*.

The industry primitives surveyed above were created primarily to defeat
DSE. In the case of EDK II, the current `ZeroMem`/`SetMem` implementation
resists DSE through its `volatile` implementation, but the lesson this RFC draws
is the structural one: a separately-named, contract-bearing clearing primitive.
That same structure is what carries the ordering and barrier guarantees
`volatile` alone does not, which is the gap this RFC is actually built on.

The DSE discussion that follows is retained as background and as the reason the
pattern exists, not as a claim that EDK II's current wipe is being deleted.

EDK II's current position only poses a performance-contracted primitive, which has
a `CrtLibSupport.h` shim that maps upstream's `SecureZeroMemory` onto it, is the
position each previously cited peer has explicitly moved away from.

Standards work and ongoing activity that motivate this proposal:

- **ISO/IEC 9899:2024 (C23), Section 7.24.6.2,
  [`memset_explicit`](https://en.cppreference.com/w/c/string/byte/memset).**
  In 2024 the C standards committee added a new, mandatory (i.e. *not*
  Annex K, *not* optional) `<string.h>` function whose entire reason for
  existing is the contract this RFC asks `SecureZeroMemoryLib` to carry.
  The standard's own footnote on it reads: *"The intention is that the
  memory store is always performed (i.e., never elided), regardless of
  optimizations. This is in contrast to calls to the `memset` function."*
  The proposal that landed it is
  [WG14 N2897](https://www.open-std.org/jtc1/sc22/wg14/www/docs/n2897.htm)
  (Miguel Ojeda, 2021); its motivation section is, in substance, the
  same argument made in this RFC. EDK II inheriting that pattern as a
  library class is consistent with the direction the language itself has
  taken.
- **Zhaomo Yang, Brian Johannesmeyer, Anders Trier Olesen, Sorin Lerner,
  Kirill Levchenko. "Dead Store Elimination (Still) Considered Harmful",
  USENIX Security 2017**
  ([full paper](https://www.usenix.org/system/files/conference/usenixsecurity17/sec17-yang.pdf)).
  Cited by WG14 N2897 above as part of the empirical justification for
  standardizing `memset_explicit`. Surveys eleven open-source security
  projects; finds four use scrubbing techniques that fail to scrub and
  another four use their scrubbing function inconsistently; documents
  that effectiveness varies across compiler versions for the same
  source code. The techniques the paper catalogues as *failing* are
  the same techniques `ZeroMem()` uses today.
- **CERT C secure coding rule
  [MSC06-C: Beware of compiler optimizations](https://web.archive.org/web/20230924213516/https://wiki.sei.cmu.edu/confluence/display/c/MSC06-C.+Beware+of+compiler+optimizations)**
  (Wayback Machine snapshot; the SEI confluence wiki has since been
  retired). Identifies `memset()` of sensitive data as a noncompliant
  pattern that compilers are entitled to remove, and names
  `SecureZeroMemory()` as the canonical Windows-platform compliant
  solution.
- **GCC bug [#8537](https://gcc.gnu.org/bugzilla/show_bug.cgi?id=8537),
  "memset() optimized away"**: Resolved as WONTFIX. Per the language
  standard the compiler is correct to remove dead stores, so a separately
  contracted API is required. This is the toolchain-side position the
  C23 committee accepted when it added `memset_explicit` rather than
  attempting to constrain `memset` itself. The conclusion has been
  stable across the ecosystem for two decades and was ratified at the
  standards level in 2024.

## Goals

1. Provide an EDK II base library interface whose contract is: *the buffer will
   be zeroed in memory, and the writes will neither be elided nor reordered away
   by the optimizer, regardless of how the call site is inlined or whole-program
   optimized.*
2. Preserve `BaseMemoryLib::ZeroMem()`'s existing performance contract
   unchanged.
3. Match the call shape of EDK II callers and ported third-party
   code to ensure that the code adoption is mechanical.
4. Permit per-architecture replacement of the default C implementation
   (e.g. an assembly trampoline) without altering the public API or its
   consumers.

## Requirements

1. A new library class, `SecureZeroMemoryLib`, MUST publish a single public
   entry point:

   ```c
   VOID *
   EFIAPI
   SecureZeroMemory (
     OUT VOID   *Buffer,
     IN  UINTN  Length
     );
   ```

   matching the header already proposed in
   [`MdePkg/Include/Library/SecureZeroMemoryLib.h`](https://github.com/tianocore/edk2/pull/12244/files).
2. The zeroing MUST execute and MUST NOT be reordered across the call
   boundary under compiler inlining, LTO, IPO, or PGO. The ordering guarantee
   (not merely the presence of the stores) is the primary contract; see the
   [Code Examples](#code-examples) section. Achieved via a combination of `noinline`,
   volatile-qualified stores, and an explicit `"memory"`-clobber compiler barrier,
   as in the implementation proposed in [`MdePkg/Library/SecureZeroMemoryLib/SecureZeroMemory.c`](https://github.com/tianocore/edk2/pull/12244/files).
3. `SecureZeroMemory()` MUST be a no-op (returning `Buffer`) when `Buffer`
   is `NULL` *or* `Length` is `0`, matching the defensive shape of the
   industry analogues.
4. The library MUST be usable from all phases.
5. The interface MUST permit alternative backend implementations (notably a
   per-architecture assembly trampoline) to be dropped in via a different
   INF without changing callers or the public header.
6. The documentation comment on `ZeroMem()` in
   [`MdePkg/Include/Library/BaseMemoryLib.h`](https://github.com/tianocore/edk2/blob/master/MdePkg/Include/Library/BaseMemoryLib.h)
   MUST be amended to explicitly state that `ZeroMem()` does not carry a
   secure-erase contract and to direct callers handling confidential
   material to `SecureZeroMemory()`. This is a documentation-only change and
   `ZeroMem()`'s signature and behavior are unaltered.

## UEFI/PI Specification Impact

None. This RFC introduces an EDK II-internal library class. It does not
modify, extend, or constrain any UEFI or PI specification interface, and
does not require a Code First issue.

## Backward Compatibility

This is an additive change.

- No existing API is modified or removed. `ZeroMem()` keeps its current
  signature, behavior, and performance characteristics.
- No callers are broken. Existing `ZeroMem()` call sites continue to
  compile and run identically.
- Initial adopters in
  [`CryptoPkg/Library/Include/CrtLibSupport.h`](https://github.com/tianocore/edk2/blob/master/CryptoPkg/Library/Include/CrtLibSupport.h)
  and
  [`TcgTpmPkg/Private/Include/Standard/CrtLibSupport.h`](https://github.com/tianocore/edk2/blob/master/TcgTpmPkg/Private/Include/Standard/CrtLibSupport.h)
  today contain:

  ```c
  #ifndef SecureZeroMemory
  #define SecureZeroMemory(ptr, sz)  memset((ptr), 0, (sz))
  #endif
  ```

  i.e. they are already calling out for this primitive and silently
  falling back to `SetMem` on EDK II builds, which erases, but carries no
  ordering contract. The proposed changes wire those calls to the real
  implementation.
- Platforms that integrate modules consuming the new class MUST add a
  library mapping to their DSC (see Platform/Package Impact, below).
  Platforms that do not consume the class are unaffected.

There is no deprecation in this RFC. A future change may consider
auditing remaining `ZeroMem()` sites that handle sensitive data, but
that is out of scope here and would not be a forced migration.

## Platform/Package Impact

Files added (proposed in [PR #12244](https://github.com/tianocore/edk2/pull/12244)):

- `MdePkg/Include/Library/SecureZeroMemoryLib.h`
- `MdePkg/Library/SecureZeroMemoryLib/SecureZeroMemoryLib.inf`
- `MdePkg/Library/SecureZeroMemoryLib/SecureZeroMemory.c`

Files modified:

- `MdePkg/MdePkg.dec`: register the new library class.
- `MdePkg/MdePkg.dsc`: provide the default implementation mapping for
  the package's own self-build.
- `MdePkg/Include/Library/BaseMemoryLib.h`: doc-comment amendment on
  `ZeroMem()` per Requirement 6 (no signature change).
- `CryptoPkg/Library/Include/CrtLibSupport.h`: replace the fallback
  `#define SecureZeroMemory ... memset(...)` with an `#include` of the
  new header.
- `TcgTpmPkg/Private/Include/Standard/CrtLibSupport.h`: same pattern.

Platform integration: platforms that build any module consuming
`SecureZeroMemoryLib` (initially: any platform building `CryptoPkg`
or `TcgTpmPkg` modules) MUST add to their DSC:

```ini
[LibraryClasses]
  SecureZeroMemoryLib|MdePkg/Library/SecureZeroMemoryLib/SecureZeroMemoryLib.inf
```

Per-architecture alternative backends (e.g. an assembly trampoline) MAY
be provided in a follow-up by adding sibling INFs or source files under the same
library directory; the public header is shared.

## Unresolved Questions

1. **Default backend vs. ASM trampoline.** Should the canonical default
   implementation under `MdePkg` remain the C-with-barriers form proposed
   here, with per-architecture ASM-trampoline backends added incrementally
   as a follow-up? Or should an ASM trampoline ship as part of this RFC's
   initial implementation? See Alternatives below for the trade-off
   surfaced by `@jaykrell`.
2. **Audit scope.** Should this RFC also commit to a tree-wide audit of
   existing `ZeroMem()` call sites that handle confidential material, or
   should that audit be tracked as separate, package-scoped follow-up work
   (recommended)?
3. **Sibling primitives.** A constant-time `SecureCompareMem` is an
   adjacent and frequently-needed primitive for the same security domain
   (defending against timing side channels in MAC/hash comparisons). It is
   intentionally out of scope for this RFC; should it be proposed as a
   separate RFC, or folded in here?

## Prior Art / Related Work

The pattern proposed here is the same pattern used by every comparable
project. The canonical references:

- **Microsoft `SecureZeroMemory`**:
  <https://learn.microsoft.com/en-us/windows/win32/api/winbase/nf-winbase-securezeromemory>.
  Documented since Windows 2000 as "a pointer to the starting address of
  the block of memory to fill with zeros. ... use this function instead
  of `ZeroMemory` when ... the application can be vulnerable to
  attack."
- **OpenBSD `explicit_bzero(3)`**:
  <https://man.openbsd.org/explicit_bzero.3>.
  Introduced specifically because "the compiler may decide that
  [`bzero`] can be removed entirely."
- **Linux kernel `memzero_explicit()`**: declared in
  [`include/linux/string.h`](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/tree/include/linux/string.h),
  introduced by commit
  [`d4c5efdb9777`](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=d4c5efdb97773f59a2b711754ca0953f24516739)
  ("random: add and use memzero_explicit()") with the rationale
  "to make sure to clear the data in the most paranoid way possible".
- **OpenSSL `OPENSSL_cleanse`**:
  [`crypto/mem_clr.c`](https://github.com/openssl/openssl/blob/master/crypto/mem_clr.c).
- **BoringSSL `OPENSSL_cleanse`**:
  [`crypto/mem.c`](https://boringssl.googlesource.com/boringssl/+/refs/heads/main/crypto/mem.c).
- **mbedTLS `mbedtls_platform_zeroize`**:
  [`library/platform_util.c`](https://github.com/Mbed-TLS/mbedtls/blob/development/library/platform_util.c).
  The header comment is worth reading in full; it walks through exactly
  the same trade-offs being debated in PR #12244.
- **libsodium `sodium_memzero`**:
  <https://doc.libsodium.org/memory_management#zeroing-memory>.
- **ISO/IEC 9899:2011 Annex K `memset_s`**: K.3.7.4.1. Standardized
  precisely to give portable C programs a guaranteed-not-elided clearing
  primitive. (Note that Annex K adoption is itself contested in the
  toolchain community, which reinforces the case for EDK II providing its own
  contract-bearing API rather than relying on the C library.)

Within EDK II, prior discussion:

- [PR #12244](https://github.com/tianocore/edk2/pull/12244) is the
  source of this RFC. The PR contains the candidate implementation referenced
  throughout.
- Review comments from `@mdkinney` raised the question this RFC is
  written to answer; review comments from `@jaykrell` (cited again
  under Alternatives below) provide compiler-team-side context that
  materially supports the proposal. `@mikebeaton`'s position that
  "`ZeroMem` itself should be guaranteed" motivated Requirement 6.

## Alternatives

### Alternative 1: Add compiler barriers inside `ZeroMem` itself

- **Pros**: One fewer API; existing call sites benefit automatically.
- **Cons**:
  1. **It fuses a security contract onto a primitive whose documented contract
     is performance, under a single name.** `ZeroMem()`/`SetMem()` promise speed;
     secure erasure wants *"always emitted and ordered against surrounding
     stores."* Merging them forces every future reader of `BaseMemoryLib` to
     reason about two contracts under one name, and leaves callers who want raw
     speed unable to get it. Every comparable project keeps these as separate,
     separately-named primitives precisely because merging them is the mistake
     being corrected.
  2. **The minimal form does not actually hold.** Simply marking
     `InternalMemSetMem` `noinline` is not sufficient under the default
     whole-program build: a `noinline` C function is still visible to LTO, so
     the compiler is not *prevented* from analyzing it, concluding it writes
     only through `Buffer`, and scheduling a following plain publish store
     across the (un-inlined) call. The ordering would again rest on optimizer
     discretion rather than on a barrier. The assembly `BaseMemoryLib` backends
     avoid the reorder not because they are out-of-line but because their bodies
     are *opaque* to the compiler — a `.nasm` object carries no LTO IR, so the
     optimizer must assume the call clobbers memory. Reproducing that opacity in
     the C instance requires `noinline` **plus** an explicit `"memory"`-clobber
     barrier **plus** `-fno-lto`/`/GL-` on the translation unit — which is
     exactly the implementation this RFC proposes for `SecureZeroMemory`.
     "Harden `ZeroMem`" therefore means "reimplement `SecureZeroMemory` inside
     `ZeroMem`," not a one-line change.
  3. **Blast radius.** The ordering gap exists only for the inlinable C instance
     (`BaseMemoryLib`); the assembly instances (`BaseMemoryLibRepStr`,
     `BaseMemoryLibSse2`, `BaseMemoryLibMmx`, `BaseMemoryLibOptDxe`,
     `BaseMemoryLibOptPei`) and the service-delegating `UefiMemoryLib`
     (`gBS->SetMem`) / `PeiMemoryLib` (`gPS->SetMem`) are already incidentally
     safe, because their wipe is an opaque out-of-line call. Hardening the shared
     primitive therefore changes code generation at the ordinary-use call sites —
     on the order of 2,000+ `ZeroMem()` and ~200 `SetMem()` sites across edk2,
     essentially all of them buffer initialization rather than secret erasure —
     and, to keep behavior uniform, the fence would have to be replicated
     consistently across *every* `BaseMemoryLib` instance, including the
     hand-written assembly ones. A separate primitive confines the change to the
     sites that actually need it and touches none of the existing callers.
  4. **It remains uncontracted.** Even after hardening, nothing *documents* that
     `ZeroMem()` orders its wipe, so the guarantee stays an implementation
     property that a later change — re-enabling LTO on the translation unit, a
     new `BaseMemoryLib` instance, a compiler-recognized clear — can silently
     remove, with no contract or test to catch the regression. The per-call cost
     of the hardening is real but hard to quantify in cycles; the decisive
     objection is architectural, not performance.

### Alternative 2: Require callers to mark buffers `volatile` themselves

- **Pros**: Zero new code.
- **Cons**: Error-prone; "viral" (the qualifier infects every pointer
  expression touching the buffer); easy to forget at one of the dozens
  of call sites. Yang et al. (USENIX 2017) measured this and found
  inconsistent effectiveness across compiler versions even when applied
  correctly.

### Alternative 3: Per-architecture assembly trampoline

- **Pros**: As `@jaykrell` notes on PR #12244, this is the strongest
  available barrier: compiler developers have stated they are not
  willing to be blind to `no-lto`, and a single-instruction assembly
  trampoline (`jmp memset`) is what production security-conscious code
  uses to keep the optimizer from seeing through the call.
- **Cons**: More code per supported architecture (IA32, X64, AARCH64,
  ARM, RISCV64, LOONGARCH64, EBC).
- **Why not chosen as the initial backend**: The library class proposed
  by this RFC is **explicitly designed to allow such a backend to be
  dropped in later as a separate INF without changing callers or the
  public header.** Landing the interface with the portable C-with-barriers
  default unblocks immediate adoption (CryptoPkg, TcgTpmPkg) and leaves
  the per-arch ASM as a follow-up that can proceed in parallel.

### Alternative 4: Do nothing; rely on present `ZeroMem`

- **Pros**: Zero work.
- **Cons**: Leaves the ordering guarantee uncontracted. `ZeroMem`/`SetMem`
  erases reliably today. Its `volatile` stores are not dead-store-eliminated.
  But nothing in its contract ensures that erasure against surrounding ordinary
  memory, and under the default whole-program build a following plain "safe to
  reuse" store can be reordered into the middle of the wipe (see [Motivation](#motivation)).
  Callers handling secrets keep depending on an incidental implementation
  property rather than a guarantee, and the upstream `SecureZeroMemory` /
  `OPENSSL_cleanse` intent remains honored only by accident.

## Implementation Design

### Architecture Overview

```mermaid
flowchart LR
  Caller["Security-sensitive caller<br/>(CryptoPkg / TcgTpmPkg / ...)"]
  Header["MdePkg/Include/Library/<br/>SecureZeroMemoryLib.h"]
  DefImpl["MdePkg/Library/SecureZeroMemoryLib/<br/>SecureZeroMemory.c<br/>(default C + barrier backend)"]
  AsmImpl["Future: per-arch ASM trampoline<br/>(sibling INF, same header)"]
  Caller -->|"SecureZeroMemory(Buf, Len)"| Header
  Header --> DefImpl
  Header -.->|"alternative backend"| AsmImpl
```

One function interface. Multiple backends permitted; one default backend in
this RFC. Public header is the single integration surface.

### Detailed Design

The proposed default implementation (full source in PR #12244) layers
three independent mechanisms, each defeating a different optimizer
behavior:

1. **`noinline` wrapper**: `SZM_NOINLINE` expands to
   `__declspec(noinline)` on MSVC and `__attribute__((noinline))` on
   GCC/Clang. This forces the optimizer to retain a real call boundary
   the IPA/LTO passes cannot trivially see through.
2. **`volatile`-qualified store loop**: writes go through
   `volatile UINT8 *`, which forbids the compiler from substituting an
   intrinsic that "knows" the post-state is dead, and prevents the
   stores within the function body from being elided.
3. **Compiler-fence barrier**: `SecureZeroMemoryBarrier(Buffer)`
   emits `_ReadWriteBarrier()` on MSVC and
   `__asm__ __volatile__("" : : "r"(Buffer) : "memory")` on GCC/Clang.
   This both reads `Buffer` (defeating "the buffer is unused" reasoning)
   and clobbers memory (defeating reordering of any post-call use). It
   is emitted both inside the noinline worker and at the public entry
   point's return path.

Combined, these mean that to drop or reorder the writes, the compiler would
have to (a) inline through a `noinline` boundary, (b) reason past a volatile
store, and (c) ignore an asm-clobber barrier. No mainstream toolchain
currently does all three; the asm-trampoline alternative remains
available as a future hardening step (Alternative 3).

### Code Examples

This gap is reproduced in-tree by a demonstration module,
[`MdeModulePkg/Test/SecureZeroMemoryDemo`](https://github.com/kuqin12/edk2/tree/sec_z_mem_example/MdeModulePkg/Test/SecureZeroMemoryDemo).

It exercises the same "wipe a secret, then publish a plain reuse flag" shape in
a `ZeroMem ()` and a `SecureZeroMemory ()` variant. Each variant is a separate
`noinline` function so it disassembles cleanly on its own; the producer and
consumer of the secret are `noinline` too, so the optimizer must treat the buffer
as genuinely written and then read; the buffer is seeded from a run-time value so
it is not constant-folded; and every result is funnelled into a `volatile` sink
so nothing the demonstration relies on is dead.

The firmware-relevant probe wipes a secret in a structure, then publishes a
plain "safe to reuse" flag (`DemoReleaseSecure ()` is identical but calls
`SecureZeroMemory ()`):

```c
typedef struct {
  UINT8     Key[32];
  UINT64    Ready;   // 64-bit field makes the struct 8-byte aligned, so ZeroMem
                     // takes its branchless path (the common case for key buffers)
} DEMO_SESSION;

UINTN
EFIAPI
DemoReleaseZeroMem (
  IN OUT DEMO_SESSION  *Session
  )
{
  UINTN  Result;

  Produce (Session->Key, sizeof (Session->Key), mDemoSeed);
  Result = Consume (Session->Key, sizeof (Session->Key));

  ZeroMem (Session->Key, sizeof (Session->Key));  // wipe the secret
  Session->Ready = 1;                             // plain "secret gone, reuse ok" publish

  return Result;
}
```

The commands used for building and disassembling the demonstration module are:

```bash
build -a X64 -t GCC -p MdeModulePkg/MdeModulePkg.dsc \
      -m MdeModulePkg/Test/SecureZeroMemoryDemo/SecureZeroMemoryDemo.inf
objdump -D Build/MdeModule/DEBUG_GCC/X64/.../SecureZeroMemoryDemo.debug -S -M intel
```

Under `-flto`, GCC inlines `ZeroMem ()` to `InternalMemSetMem`'s branchless 64-bit
store loop and then hoists the plain `Session->Ready = 1` *ahead of the entire
wipe*, where `Ready` is published while all 32 secret bytes are still resident (`rax`
holds `0`; `[rbx+0x20]` is `Ready`, `[rbx]` is `Key`):

```asm
  ZeroMem (Session->Key, sizeof (Session->Key));
  Session->Ready = 1;
    1ca9:  mov QWORD PTR [rbx+0x20], 0x1   ; Ready = 1        <-- published first
    1cb1:  mov QWORD PTR [rbx],      rax   ; wipe Key[0:8)   = 0
    1cb4:  mov QWORD PTR [rbx+0x8],  rax   ; wipe Key[8:16)
    1cb8:  mov QWORD PTR [rbx+0x10], rax   ; wipe Key[16:24)
    1cbc:  mov QWORD PTR [rbx+0x18], rax   ; wipe Key[24:32)
```

`DemoReleaseSecure ()` is identical except it calls `SecureZeroMemory ()`, which
stays a real out-of-line call and pins the publish strictly after the wipe:

```asm
  SecureZeroMemory (Session->Key, sizeof (Session->Key));
    1f06:  call 2115 <SecureZeroMemory>    ; out-of-line barrier (noinline, -fno-lto)
  Session->Ready = 1;
    1f0b:  mov QWORD PTR [rbx+0x20], 0x1   ; Ready = 1 - after the completed wipe
```

The reorder is permitted by ISO C (§5.1.2.3: `volatile` accesses are ordered only
relative to one another) and is taken by GCC here in an actual EDK II build; a
contract-bearing `SecureZeroMemory ()` removes the permission uniformly. The
reorder surfaces because the wipe reached `InternalMemSetMem`'s *branchless* 8-byte
path, which GCC selects when the buffer is provably 8-byte aligned.

The exposure is specific to the inlinable C `BaseMemoryLib`. Other optimization
instances such as `BaseMemoryLibRepStr` keep `ZeroMem ()` an opaque out-of-line
`call` through assembly body, so `Ready` is pinned after the completed wipe and
the reorder disappears. The service-delegating `UefiMemoryLib` (`gBS->SetMem`) and
`PeiMemoryLib` (`gPS->SetMem`) are safe for the same reason.

**Canonical caller pattern:**

```c
#include <Library/SecureZeroMemoryLib.h>

EFI_STATUS
EFIAPI
DeriveAndUseKey (
  IN  CONST UINT8  *Seed,
  IN  UINTN        SeedLength,
  OUT UINT8        *Output,
  IN  UINTN        OutputLength
  )
{
  UINT8  Key[32];
  EFI_STATUS  Status;

  Status = DeriveKeyFromSeed (Seed, SeedLength, Key, sizeof (Key));
  if (EFI_ERROR (Status)) {
    SecureZeroMemory (Key, sizeof (Key));
    return Status;
  }

  Status = EncryptWithKey (Key, sizeof (Key), Output, OutputLength);

  //
  // Wipe the key on every exit path, including success.
  //
  SecureZeroMemory (Key, sizeof (Key));
  return Status;
}
```

Platform DSC integration:

```ini
[LibraryClasses]
  SecureZeroMemoryLib|MdePkg/Library/SecureZeroMemoryLib/SecureZeroMemoryLib.inf
```

## Testing Strategy

- **Build verification** across all supported toolchains and
  architectures in TianoCore CI (the existing
  `MdePkg.ci.yaml` / `MdePkg.dsc` matrix), confirming the new INF
  compiles without warnings under `-Werror`-class settings.
- **Host-based unit test** (under `UnitTestFrameworkPkg`) that calls
  `SecureZeroMemory()` on initialized buffers of varying alignment and
  length (including `Length == 0` and `Buffer == NULL`) and asserts
  the post-call contents. This verifies that the *contract* of "the buffer is
  observably zero on return".
- **Disassembly inspection**, performed manually during development on
  the supported toolchains, confirming that the wipe is emitted and is not
  reordered across a following store at default optimization level in `CryptoPkg`.
- **What we deliberately do not promise**: a CI test that "proves"
  optimization-induced elision or reordering on a specific compiler build. As Yang
  et al. document, such a test would be a brittle indicator of one
  toolchain version's behavior, not a stable measurement of the
  library's contract. The guarantee provided by this library is
  *structural* (noinline + volatile + barrier), and is validated by
  the structure of the implementation rather than by empirical
  observation against a snapshot toolchain.

## Migration / Adoption Plan

1. **Phase 1: Land the library and initial adopters.** Merge
   [PR #12244](https://github.com/tianocore/edk2/pull/12244) once this
   RFC is accepted:
   - New `SecureZeroMemoryLib` class and default backend in `MdePkg`.
   - `CryptoPkg` and `TcgTpmPkg` `CrtLibSupport.h` switched from the
     `#define SecureZeroMemory ... memset(...)` fallback to the real
     library.
   - `ZeroMem()` doc-comment amendment in
     `MdePkg/Include/Library/BaseMemoryLib.h` per Requirement 6.
2. **Phase 2: Opportunistic adoption.** Package maintainers audit and
   convert `ZeroMem()` call sites that handle confidential material
   in their packages, scoped per-package, on their own schedule.
   Suggested initial targets: `SecurityPkg`,
   `MdeModulePkg/Library/AuthVariableLib`, `NetworkPkg/Tls*`,
   `CryptoPkg/Library/BaseCryptLib` direct callers.

Risk mitigation: each step is additive and gated by individual PR
review. The absence of adoption in any given package is functionally equivalent
to today, not a regression.

## Guide-Level Explanation

### For Package Developers

Use `SecureZeroMemory()` instead of `ZeroMem()` when the buffer
contains material whose disclosure would constitute a security failure:

- Cryptographic keys, key schedules, IVs, nonces in their secret form.
- Plaintext that has just been decrypted.
- TPM authorization values, session keys, HMAC keys.
- User credentials, passwords, PINs.
- RNG internal state.
- Any buffer whose contents you are wiping *because they are sensitive*
  rather than *because the next use needs zero-initialized memory*.

Use `ZeroMem()` elsewhere. The distinction is the *reason* for
the initialization vs. the erasure.

### For Platform Developers

Add the library mapping to your platform DSC:

```ini
[LibraryClasses]
  SecureZeroMemoryLib|MdePkg/Library/SecureZeroMemoryLib/SecureZeroMemoryLib.inf
```

Should a future RFC add a per-architecture ASM-trampoline backend, a new INF
path can be used to opt into it.

### For End Users

No visible behavior change.
