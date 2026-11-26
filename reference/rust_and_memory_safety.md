# Rust and Memory Safety

```admonish tip title="Current state"
Jump to [Current state (November 2026)](#current-state-november-2026) for the latest status.
```

## Table of Contents

- [Background](#background)
- [Before TianoCore Efforts: Rust Had UEFI Target Support](#before-tianocore-efforts-rust-had-uefi-target-support)
- [2019 to 2020: The edkii-rust Experimental Branch](#2019-to-2020-the-edkii-rust-experimental-branch)
  - [Branch Overview](#branch-overview)
- [August 2020: UEFI Plugfest and Open Source Firmware Conference (OSFC)][osfc-2020-in-doc]
- [2022: The Toolchain Gap and Google Summer of Code](#2022-the-toolchain-gap-and-google-summer-of-code)
- [2023: Project Mu publicly leverages the edk2-staging work](#2023-project-mu-publicly-leverages-the-edk2-staging-work)
- [2024: UEFI Rust code ships](#2024-uefi-rust-code-ships)
- [Patina: UEFI Rust from the core up](#patina-uefi-rust-from-the-core-up)
  - [Origin](#origin)
  - [Patina Background](#patina-background)
  - [Value of starting from the core](#value-of-starting-from-the-core)
  - [How it relates to the other Rust projects](#how-it-relates-to-the-other-rust-projects)
- [2026: Increased pressure on C code from AI](#2026-increased-pressure-on-c-code-from-ai)
- [Current state (November 2026)](#current-state-november-2026)
- [Appendix: Rust in Android](#appendix-rust-in-android)
  - [April 2021: Rust joins the Android platform](#april-2021-rust-joins-the-android-platform)
  - [December 2022: Significant drop in memory safety vulnerabilities](#december-2022-significant-drop-in-memory-safety-vulnerabilities)
  - [November 2025: Memory safety vulnerabilities below 20 percent](#november-2025-memory-safety-vulnerabilities-below-20-percent)
- [Summary of related reading](#summary-of-related-reading)

This document captures the history and evolution of TianoCore community member efforts in writing UEFI code in Rust with
EDK II interoperability with EDK II up to the current state. The document is meant to clarify how various efforts you
may find in the ecosystem relate to and have built upon each other to advance memory safety in UEFI development.

The sections follow a chronological progression, outlining the challenges that motivated each effort, the work
undertaken to address them, and how that work informed subsequent efforts.

## Background

Most UEFI firmware, including EDK II, is written in C. C gives the programmer direct control over memory and almost no
protection when that control is misused. Reading past the end of a buffer, using memory after it was freed, or letting
an integer wrap around and then using it as a size are all ordinary mistakes in C, and the language does not stop them.
Bugs like these are called memory safety bugs, and they are a common source of security vulnerabilities.

Rust is a systems programming language that addresses many common memory safety errors through compile-time checks and
runtime validation. Two central concepts are **ownership** and **borrowing**. Ownership ties each value to an owner,
with resources normally released automatically when the owner goes out of scope, reducing the need for manual memory
management. Borrowing allows code to access a value through references without taking ownership. The compiler checks
that references do not outlive the values they refer to and enforces rules governing shared and mutable access. Array
and slice indexing also includes bounds checks, so an out-of-range index triggers a panic rather than an invalid memory
access.

Rust still requires careful design and review. Operations such as dereferencing raw pointers require `unsafe` code,
where developers must uphold safety requirements that the compiler cannot verify. Firmware commonly needs these
operations to access hardware and interact with C interfaces. The aim throughout all of the Rust work, has been to some
extent, to confine `unsafe` code in small, reviewable implementations that expose safe interfaces to the rest of the
code. Over time, we have become more adept at identifying which parts of the code truly need `unsafe` and at minimizing
its scope, thereby improving overall safety and maintainability. In addition to more experienced with applying Rust
language features and idioms into cleaner and safer abstractions.

Rust's library support is organized into layers. `core` provides fundamental types and functionality without requiring
an operating system. `alloc` adds heap-allocated types such as `Vec` and `String`, using an allocator supplied by the
binary. `std` provides higher-level facilities, including file access, threads, and networking, whose availability
depends on the target environment. UEFI Rust code commonly uses `no_std`, relying on `core` and optionally `alloc`
rather than `std`. In general, UEFI `std` has been largely considered more relevant to UEFI application as opposed to
other module type code.

UEFI also has specific binary and interface requirements. Executable images use the PE/COFF format, and the entry point
receives access to firmware services through tables of function pointers. UEFI text interfaces use UTF-16 strings. Rust
provides the `efiapi` calling convention to match the UEFI ABI, which differs from the default C calling convention on
some architectures.

## Before TianoCore Efforts: Rust Had UEFI Target Support

Before TianoCore members began experimenting with Rust, the Rust compiler already supported building UEFI binaries. This
support provided a foundation for compiling Rust code for UEFI independently of the EDK II build system. The
[Rust platform support documentation][rustc-uefi] lists three UEFI targets:

- `x86_64-unknown-uefi` for 64-bit x86 systems.
- `i686-unknown-uefi` for 32-bit x86 systems.
- `aarch64-unknown-uefi` for 64-bit Arm systems.

These are cross-compilation targets: developers run the compiler on a supported host system to produce binaries for the
UEFI environment. Their default configuration reflects several firmware requirements:

- **Image format and linking:** LLVM's `lld` linker produces a PE/COFF executable with an `.efi` extension. Code is
  statically linked rather than relying on a dynamic linker.
- **Entry point and image type:** The default entry point is `efi_main`, and the default image subsystem is
  `EFI_APPLICATION`. Linker options can select a different entry point or produce a boot service driver or runtime
  driver instead.
- **Calling convention:** Rust's `efiapi` ABI selects the calling convention required by UEFI for the target
  architecture. It is used when declaring entry points and firmware interfaces.
- **Panic handling:** The default panic strategy is `abort` rather than stack unwinding. In a `no_std` binary, the
  program supplies a panic handler to define what happens when a panic occurs.
- **Processor state:** Floating-point and vector features such as MMX and SSE are disabled by default so that code can
  run before firmware initializes the corresponding processor state. These defaults can be overridden when the execution
  environment supports those features.

The UEFI targets are currently classified as [Tier 2][rustc-tier-2]. Rust's release infrastructure checks that they
build, but does not require the same automated execution testing as Tier 1 targets. Also note that this classification
describes compiler and library support, not validation of a particular firmware image on hardware.

Community libraries grew around those targets:

- [r-efi][r-efi] is a direct transposition of the UEFI specification into Rust. It provides constants and type
  definitions with no helpers on top, which makes it a base other projects can build on. Its repository dates to
  November 2018 and it has been used throughout various efforts as an underlying foundation for UEFI support in Rust
  projects.
- [uefi-rs][uefi-rs] is a safe wrapper for building UEFI applications. It establishes an allocator and an execution
  environment so ordinary Rust code can call firmware services with less `unsafe` code.

## 2019 to 2020: The edkii-rust Experimental Branch

Branch: [edk2-staging/edkii-rust](https://github.com/tianocore/edk2-staging/tree/edkii-rust)

In November 2019 the Microsoft Security Response Center (MSRC) published a post about [using Rust in Windows][msrc-rust]
that is cited in the edkii-rust branch README as a motivation for the work.

A [February 2019 Microsoft talk][2019-trends-chlgs-sw] also focused on vulnerability trends. In Microsoft's software,
memory safety issues contributed to approximately 70 percent of recent security issues.

In preparation for the 2020 Enabling Rust for UEFI Firmware talk, Jiewen Yao and Vincent Zimmer noted the following
share of root causes for security issues in EDK II.

| Top Issue                          | Open Source | Closed Source |
| ---------------------------------- | ----------- | ------------- |
| Buffer Overflow / Integer Overflow | 50%         | 38%           |
| SMM                                | 7%          | 18%           |
| UEFI Variables                     | 8%          | 5%            |
| Register Locks                     | 3%          | 10%           |

The data was convincing enough that memory safety was a significant concern. However, since EDK II is a very large C
code base, and a full rewrite was impractical, so a strategy to support an incremental migration was needed.

Since this was an effort with minimal resources that needed to gradually support Rust adoption alongside the C code, a
branch to host experimental code was created in edk2-staging called "edkii-rust". This was primarily developed by Jiewen
Yao and Xiaoyu Lu at Intel.

That work is still available in the [edkii-rust branch of edk2-staging][edkii-rust]. It is old now being based on the
`edk2-stable201911` tag. Note that as the README states, it is an early experiment and should not be used for production
workloads. The last commit under its `RustPkg` directory is from January 2020.

### Branch Overview

The code was created under `RustPkg`. A key part of work at the time was integrating Rust into the existing EDK II build
tools and build process.

- **Build rules.** A Rust source file could be built into a library with a rule that turns `.rs` into `.lib`. A
  `Cargo.toml` listed in an INF file could be built into either a library or a complete `.efi` module. Rust's dependency
  mechanism, Cargo, handled Rust-to-Rust dependencies, and the INF file's library dependencies handled Rust-to-C
  dependencies. Six combinations of C and Rust modules and libraries. There were various limitations for different
  combinations.
  > - C source + Rust source mixed in INF (Library or Module)
  >   - Rust source code is supported by EDKII build rule – Rust-To-Lib-File (.rs => .lib)
  >   - Limitation: Rust cannot have external dependency.
  > - Pure Rust Module only.
  >   - A Cargo.toml file is added to INF file as source.
  >   - Rust Module build is supported by EDKII build rule – Toml-File.RUST_MODULE (Toml => .efi)
  >   - Limitation: Runtime might be a problem, not sure about virtual address translation for rust internal global
  >     variable.
  > - Pure Rust Module + Pure Rust Library with Cargo Dependency.
  >   - The cargo dependency means the rust lib dependency declared in Cargo.toml.
  > - Pure Rust Module + C Library with EDKII Dependency.
  >   - Rust Module build is supported by EDKII build rule – Toml-File (Toml => .lib)
  >   - The EDKII dependency means the EDKII lib dependency declared in INF.
  >   - If a rust module is built with C, the cargo must use staticlib. Or rlib should be used.
  > - C Module + Pure Rust Library with EDKII Dependency.
  >   - Rust Lib build is supported by EDKII build rule – Toml-File. (Toml => .lib)
- **Packaging.** Running the normal `build` command against `RustPkg.dsc` also added the section, FFS, and firmware
  volume headers, so a Rust PEIM, DXE driver, or SMM driver could be placed in a firmware image like other modules.
- **A runtime support layer.** Rust data structures such as `Vec` need a global allocator. `UefiRustAllocationLib`
  provided one on top of the UEFI memory services. Rust also requires a panic handler, so `UefiRustPanicLib` supplied a
  default instance.
- **Toolchain.** The branch needed a nightly Rust compiler, `cargo-xbuild` to build the `core` library for the UEFI
  target, LLVM 9 for the C code with the `CLANGPDB` tool chain, and `nasm` for assembly.

In addition, there were a handful of proof-of-concept Rust libraries such as a FAT file system library, helpers for
memory allocation, debug logging, and boot services, a CHAR16 string helper, and a random number library built on the
`RDRAND` and `RDSEED` instructions. Some work was done to extend the `ring` and `webpki` crates, which provide general
purpose cryptography and certificate handling, to work in UEFI. A related branch rewrote the UEFI variable policy
library in Rust with unit tests.

Around this time, crates to provide UEFI Specification defintions were not as mature and there were not crates providing
Platform Initialization (PI) Specification definitions.

## August 2020: UEFI Plugfest and Open Source Firmware Conference (OSFC)

On August 20, 2020, Jiewen Yao and Vincent Zimmer (Intel) presented [Enabling Rust for UEFI Firmware][rust-plugfest-pdf]
at the UEFI 2020 Virtual Plugfest. They gave a similar presentation at the Open Source Firmware Conference in December.
The [Vimeo recording][rust-plugfest-video] is public. The OSFC [abstract][osfc-2020] summarizes the main ideas of the
talk.

The talk walked through the classes of memory bugs that show up in UEFI code and matched each to how Rust handles them.

- Use after free and double free are handled by ownership rules checked at compile time.
- Uninitialized variables are rejected at compile time.
- Buffer overflows and over-reads are caught by bounds checks during execution, and the failure goes to the panic
  handler.
- Null pointers are modeled with an `Option` type that forces the code to handle the empty case.
- Integer overflow is caught at run time in debug builds. In release builds the default is to wrap, unless the
  `overflow-checks` compiler flag is turned on. Explicit `checked`, `saturating`, and `wrapping` arithmetic functions
  exist for code that wants to choose.

The talk said bounds checks still need handling code, because a panic is a failure the system has to survive. Like in
the C code, keeping the system resilient requires careful error handling. It also listed problems Rust does not solve
such as silicon register locks, security policy mistakes, time-of-check to time-of-use races, and calls out of SMM.

It made the point that putting C-style code inside an `unsafe` is not valuable when it is not used as as the foundation
for safe abstractions. The call to action asked firmware developers to write critical modules in Rust.

## 2022: The Toolchain Gap and Google Summer of Code

By 2022 there were several ways to run Rust on UEFI, but none of them had been fully reconciled with EDK II.

A TianoCore Google Summer of Code project focused on improving Rust support for UEFI development. The project aimed to
provide a pathway for using Rust's `std` library in UEFI applications. The student was Ayush Singh, a second-year
undergraduate student at the Indian Institute of Technology Dhanbad, and the project was mentored by Mike Kinney and
Michael Kubacki with the following goals defined:

1. Get most of Rust-std running under the UEFI target
2. Add testing support for UEFI target
3. Pass all the general tests of library/std
4. Possibly add support for ARM and RISCV UEFI targets (currently, only X86_64, AARCH64, and I686 are present)

In the beginning it was not expected that a full `std` library would be achievable by the end of the project given the
scope and project duration.

Throughout the project, Ayush kept a blog. The repository is no longer active (as of 2022), but the the patches for
`std` support were pushed to [tianocore/rust](https://github.com/tianocore/rust). A summary of the posts can be found
[in this post][gsoc-summary].

A summary of the work included:

- Adding new UEFI Specification definitions to the r-efi repository (still used today)
- Work contributed upstream to add `std` support for x86_64-unknown-uefi targets
  - [Overall Tracking Issue for uefi-std](https://github.com/rust-lang/rust/issues/100499)
  - [Original PR](https://github.com/rust-lang/rust/pull/100316)
  - [Minimal/Merged PR](https://github.com/rust-lang/rust/pull/105861) (merged September 24, 2023)
- Found several UEFI target bugs.

The work has continued to grow and evolve as shown in the tracking issue.

`std` support maps cleanly onto UEFI Specification defined APIs. Memory allocation on `AllocatePool()` and `FreePool()`,
standard input and output use the Simple Text protocols, and arguments come from the loaded image's load options.

Rust 1.67 precompiled artifacts for the UEFI targets can be installed with `rustup target add`, so `no_std` developers
no longer need to build the compiler themselves.

The result is a safer and more ergonomic way to write UEFI applications and some drivers in Rust. However, it does not
change how a platform's DXE phase is built and how the services uefi-std is abstracted over are implemented. As
developers working in the core environment and low-level DXE modules, working in `std` is not possible because that is
what is ultimately contributing to making uefi-std work.

## 2023: Project Mu publicly leverages the edk2-staging work

Project Mu is the open source UEFI core derived from EDK II that Microsoft started in 2018. There had been some prior
discussions with Jiewen Yao about how to extend the work in the edkii-rust branch that led to Project Mu work building
upon that effort. On September 1, 2023, the Project Mu team announced initial support for Rust in this
[announcement][mu-rust]. Similar to the edkii-rust branch, Rust code was placed direclty in an EDK II style workspace
with BaseTools enlightened to understand and build modules containing varying degrees of Rust code. Details are in the
[Mu Rust build documentation][mu-rust-build].

The motivation and plan to adopt Rust for UEFI firmware was described in this [Surface IT Pro blog post][surface-itpro]
in November 2023.

During this work, the tools and Rust build process evolved, picking up [`cargo-make`][cargo-make] to simplify Rust build
orchestration and reduce custom tooling. Rust modules being built were included in Mu QEMU CI testing and were more
rigorously tested and validation in platform self-host and product builds. The engineers invovled in this work
contributed additional content to r-efi related to the UEFI Specification and created a new crate for the Platform
Initialization (PI) Specification called [mu_rust_pi][mu-rust-pi] ([crate][mu-rust-pi-crate]).

This moved Rust from a branch that showed it could work to build infrastructure that real platform teams could use.

Some Rust pacakges developed during this period include:

- [UefiHidDxeV2](https://github.com/microsoft/mu_plus/tree/release/202511/HidPkg/UefiHidDxeV2) - Consumes lower-level
  HID device support via the HidIo protocol abstraction and produces the UEFI spec input protocols for console support.
- [HidIo](https://github.com/microsoft/mu_plus/tree/release/202511/HidPkg/Crates/HidIo) - A protocol that provides
  access to HID devices.
- [HiiKeyboardLayout](https://github.com/microsoft/mu_plus/tree/release/202511/HidPkg/Crates/HiiKeyboardLayout)
  - Provides support for a Rust wrapper around UEFI HII Keyboard Layout structures. The relevant structures defined in
    the UEFI spec are not well-suited for direct definition in rust, so this crate defines analogous rust structures and
    provides serialization/deserialization support for converting the rust structures into byte buffers and vice versa.
- [A Rust crate for parsing HID report descriptors](https://github.com/microsoft/mu_rust_hid)
  ([crate](https://crates.io/crates/hidparser))

## 2024: UEFI Rust code ships

Ultimately, this resulted in the `UefiHidDxeV2` crate being part of the first known set of UEFI Rust code from this
overall effort to ship in products around April 2024.

This presented a fork in where to go next, it would have been simpler to continue with the C and Rust hybrid model and
write more code in Rust. But several key limitations were becoming apparent:

1. Mixing C and Rust code in the same EDK II workspace caused:
   - Conflicts between the two build systems and toolchains, making it difficult to manage dependencies and compile the
     firmware reliably.
   - Conflicts with how Rust expects source code to be organized versus EDK II packaging conventions.
   - Everyone working in the EDK II (predominantly C) workspace to maintain Rust build tools.
   - Confusion about how the C and Rust code integrated in different combinations. How linking worked and concerns about
     degrading the value of memory safety by blurring lines between C and Rust code loosely in each module.
   - Confusion about custom extensions to the EDK II build infrastructure and how those worked and whether we wanted to
     have a long-term path that involved tight coupling between the custom EDK II build infrastructure and what is
     otherwise a standard Rust build process.
2. The core services being used by the Rust code were still written in C, such as the UEFI Boot Services. This meant any
   Rust code was written was ultimately a relatively thin layer on C code.
3. Rust code is published through crates. It was awkward to publish crates from within an EDK II workspace, as the
   crates were difficult to find in the EDK II workspace structure.

Instead of writing more code in Rust using the hybrid C and Rust model, the team of developers active in open-source
development in this lineage of code made the decision to more fully explore a completely Rust-based DXE core, leading
to the creation of Patina.

## Patina: UEFI Rust from the core up

### Origin

Patina began as a Microsoft internal hackathon project around September 2021 started by Michael Kubacki and John Schock
using the edkii-rust branch as a reference. Like that branch, this effort was an early proof-of-concept to determine how
feasible it would be to implement a DXE core entirely in Rust. The first Hackathon work was able to demonstrate a
minimal DXE load to driver dispatch. By September 2022, the project had progressed to successfully booting to the EFI
shell. In April 2024, the project was able to boot to Windows on X64 systems for the first time.

The public [repository][patina-repo] was created in the Open Device Partnership organization in October 2024 and is open
source under the Apache 2.0 license. Patina was first presented by Michael Kubacki at the 2025 UEFI Plugfest
([slides][patina-plugfest-pdf] and [video][patina-plugfest-video]). On June 25, 2026, the UEFI Forum hosted a webinar
called [The State of Rust in UEFI and Q&A][patina-brighttalk], where Michael Kubacki, Oliver Smith-Denny, and Joey
Vagedes shared lessons from years working on Rust UEFI development. A [recording][patina-brighttalk-video] is available.

### Patina Background

As noted in [Patina's background][patina-doc], it is an effort to evolve and modernize UEFI firmware. A few design
decisions follow directly from the earlier history.

- **The core is written in Rust.** Libraries like `uefi-rs` and Rust's `std` wrap services that some other core
  provides, which in practice meant a C core. Patina implements the core itself. It also provides an SDK crate,
  `patina`, for building on top of it. As of November 2026, Patina implements a DXE Core compatible with AARCH64 and X64
  in addition to a Standalone MM Core with Supervisor capabilities for X64.
- **Components are built together.** An EDK II platform builds every driver as its own PE/COFF binary. Patina compiles
  the core and its components into a single binary, which lets the compiler follow references between them, this is
  especially important for ensuring memory safety across component boundaries.
- **Existing drivers keep working.** The Patina core dispatches PI drivers as before, so a platform can port drivers to
  Patina components over time instead of all at once. This is the same incremental idea behind the 2020 branch, applied
  to the core instead of one driver at a time.
- **Services replace global function pointer tables.** In EDK II, Boot Services is a table of function pointers
  reachable through a global pointer. The [memory safety strategy in Patina][patina-memory-safety] points out that
  memory corruption can overwrite such pointers, that nothing checks them at compile time, and that third-party drivers
  have been observed patching them. Patina components receive services as Rust traits that the compiler checks.

### Value of starting from the core

Starting from the core has many advantages.

1. In general, it contains more code than any other module. Which is now in Rust.
2. A typical platform has hundreds of DXE drivers, which interact heavily with the core through UEFI Boot Services. By
   having those services be implemented in Rust, the C drivers do not need to change at all and can still benefit from
   the memory safety guarantees provided by Rust.
3. The core implements and enforces security technologies integral to the safety of boot such as setting up the page
   table, enforcing stringent paging policies, and implementing memory mitigations.
4. Starting from a new core, language aside, provides the opportunity for modernization and elimination from the start
   of legacy patterns and technical debt.
5. The core provides a natural point to build into a .efi binary that additional Rust code can be compiled with to
   support compile-time memory safety checks across all Rust code linked into the binary.
6. Because the core provides services used by all other code in the execution phase (e.g. DXE), by writing the core in
   Rust, it can provide native, idiomatic Rust interfaces to all other code that consumes its services.

### How it relates to the other Rust projects

This is covered in an [ecosystem comparison][patina-ecosystem] section in the Patina book. At a high-level, `r-efi`
provides raw bindings, `uefi-rs` and Rust `std` provide safe application-level APIs, and Patina is a high-level
framework for a full firmware implementation in Rust. These are not competitors in most cases. Patina uses `r-efi` for
its specification definitions, and `uefi-rs` and `std` provide safety and ergonomics on top of a core written in C or in
Rust because the Boot Services interface is the common contract.

It is recommended to review the [case study document][patina-case-studies] that maps past EDK II CVEs to the Rust
features that would have helped prevent them.

## 2026: Increased pressure on C code from AI

In addition to the already noted long-term cost of memory safety bugs, AI-driven vulnerability discovery is adding
immediate pressure fixes needed to address vulnerabilities in the C code.

AI models have become effective at finding memory safety vulnerabilities in mature, widely deployed C and C++ code.
While this is not specific to UEFI, EDK II has experienced a significant increase in reported and discovered
vulnerabilities, particularly those that are AI-assisted as of November 2026. At this point, finding bugs is much faster
than fixing them, especially when deep domain knowledge is required, as is the case in various parts of the EDK II code
base. For example, as of October 2026, vulnerabilities in NetworkPkg have captured widespread attention and received
broader media coverage.

## Current state (November 2026)

Today, developers who want to do UEFI work in Rust at the same scope as EDK II are encouraged to work in
[Patina][patina-repo]. Patina is where the experience of the earlier efforts is being applied, and many of the
developers involved in EDK II now contribute to it.

The TianoCore project does not believe this is quite the right time to fully host Rust code in TianoCore. The reasons
are the current level of expertise in the Rust programming language among contributors and the infrastructure needed to
support Rust development. TianoCore and Patina will work together to continue to advance and evolve UEFI firmware
development from the ground up in memory safe languages like Rust.

Here's a final summary of some of the pieces described in this document:

| Effort                        | Purpose and Scope                                                | Summary                                                   |
| ----------------------------- | ---------------------------------------------------------------- | --------------------------------------------------------- |
| `edkii-rust` branch           | Build rules and libraries for Rust modules in the EDK II build   | A 2020 experiment, last `RustPkg` commit January 2020     |
| Rust `*-unknown-uefi` targets | Compiler support for PE32+ UEFI images                           | The foundation every other Rust effort builds on          |
| Rust `std` for UEFI           | A port of the standard library to those targets                  | Applications and some drivers, still incomplete           |
| `r-efi`                       | Raw type and constant definitions from the UEFI specification    | Shared base used by `uefi-rs` and Patina                  |
| `uefi-rs`                     | Safe wrappers for UEFI services                                  | Applications and drivers on top of a C or Rust core       |
| Project Mu Rust support       | Cargo-based Rust modules in a product build                      | Rust components alongside a C DXE core                    |
| Patina                        | A DXE core and SDK written in Rust, with PI driver compatibility | Full execution environment, where TianoCore points people |

---

## Appendix: Rust in Android

The ongoing success of Rust in the Android ecosystem continues to motivate the adoption of Rust in other low-level
systems programming domains, including UEFI firmware development. We have been following its progress for a few years
now.

Android is a useful comparison because it is a large code base with a lot of C and C++ in low-level system components,
and because the Android team has published data on the impact of adopting Rust in their code base over time.

### April 2021: Rust joins the Android platform

On April 6, 2021, Jeff Vander Stoep and Stephen Hines announced in [Rust in the Android platform][android-2021] that the
Android Open Source Project now supports Rust for developing the operating system itself. The post opened with the
problem:

> Correctness of code in the Android platform is a top priority for the security, stability, and quality of each Android
> release. Memory safety bugs in C and C++ continue to be the most-difficult-to-address source of incorrectness. We
> invest a great deal of effort and resources into detecting, fixing, and mitigating this class of bugs, and these
> efforts are effective in preventing a large number of bugs from making it into Android releases.

It went on to say that memory safety bugs still "consistently represent ~70% of Android's high severity security
vulnerabilities". Three arguments in the post apply to firmware as well.

### December 2022: Significant drop in memory safety vulnerabilities

On December 1, 2022, Jeff Vander Stoep followed up with [Memory Safe Languages in Android 13][android-2022]. The post
began with this observation:

> For more than a decade, memory safety vulnerabilities have consistently represented more than 65% of vulnerabilities
> across products, and across the industry. On Android, we're now seeing something different - a significant drop in
> memory safety vulnerabilities and an associated drop in the severity of our vulnerabilities.

The annual number of memory safety vulnerabilities in the Android security bulletin dropped from 223 in 2019 to 85
in 2022. Their share of all Android vulnerabilities fell from 76 percent to 35 percent, and 2022 was the first year they
were not a majority. Android 13 was the first release where a majority of new code was written in a memory safe
language. About 21 percent of new native code in that release was Rust, roughly 1.5 million lines in total, and the
authors reported zero memory safety vulnerabilities found in it so far. They did not expect that number to stay zero.

The authors noted that some other factors may have contributed such a C and C++ hardening tools that were adopted.

### November 2025: Memory safety vulnerabilities below 20 percent

On November 13, 2025, Jeff Vander Stoep published [Rust in Android: move fast and fix things][android-2025]. Its
headline result extends the 2022 trend. The post said the 2025 data "continues to validate the approach, with memory
safety vulnerabilities falling below 20% of total vulnerabilities for the first time."

- **Vulnerability density.** Using roughly 5 million lines of Rust in the Android platform and one potential memory
  safety vulnerability found before release, the authors estimated 0.2 vulnerabilities per million lines. They put the
  historical figure for Android's C and C++ code closer to 1,000 per million lines, which is a more than 1000x
  difference.
- **Delivery speed.** For medium and large changes, Rust changes had about a 4x lower rollback rate than C++ changes and
  spent about 25 percent less time in code review.

The post also mentioned that the project has been writing firmware in Rust for years and that moving firmware to Rust
can yield a major improvement in security,

## Summary of related reading

- [Enabling Rust for UEFI Firmware (slides, August 2020)][rust-plugfest-pdf]
- [The edkii-rust branch in edk2-staging][edkii-rust]
- [Rust platform support for `*-unknown-uefi`][rustc-uefi]
- [Tracking issue for UEFI std in Rust][rust-100499]
- [Patina background][patina-doc]
- [Patina in the UEFI Rust ecosystem][patina-ecosystem]
- [Patina DXE core memory strategy][patina-memory-safety]
- [Real world case study: UEFI memory safety issues preventable by Rust][patina-case-studies]
- [Rust in the Android platform (April 2021)][android-2021]
- [Memory Safe Languages in Android 13 (December 2022)][android-2022]
- [Rust in Android: move fast and fix things (November 2025)][android-2025]

[osfc-2020-in-doc]: #august-2020-uefi-plugfest-and-open-source-firmware-conference-osfc
[rustc-uefi]: https://doc.rust-lang.org/rustc/platform-support/unknown-uefi.html
[rustc-tier-2]: https://doc.rust-lang.org/1.99.1/rustc/platform-support.html#tier-2-without-host-tools
[r-efi]: https://github.com/r-efi/r-efi
[uefi-rs]: https://github.com/rust-osdev/uefi-rs
[msrc-rust]: https://msrc-blog.microsoft.com/2019/11/07/using-rust-in-windows/
[2019-trends-chlgs-sw]:
  https://github.com/microsoft/MSRC-Security-Research/blob/master/presentations/2019_02_BlueHatIL/2019_01%20-%20BlueHatIL%20-%20Trends%2C%20challenge%2C%20and%20shifts%20in%20software%20vulnerability%20mitigation.pdf
[edkii-rust]: https://github.com/tianocore/edk2-staging/tree/edkii-rust
[rust-plugfest-pdf]:
  https://uefi.org/sites/default/files/resources/Enabling%20RUST%20for%20UEFI%20Firmware_8.19.2020.pdf
[rust-plugfest-video]: https://vimeo.com/488129544
[osfc-2020]: https://www.osfc.io/2020/talks/enabling-rust-for-uefi-firmware/
[gsoc-summary]: https://programmershideaway.dev/posts/post15/
[rust-100499]: https://github.com/rust-lang/rust/issues/100499
[surface-itpro]:
  https://techcommunity.microsoft.com/blog/surfaceitpro/rust-support-for-uefi-development-through-project-mu/3970582
[mu-rust]: https://microsoft.github.io/mu/WhatAndWhy/rust/
[mu-rust-pi]: https://github.com/microsoft/mu_rust_pi
[cargo-make]: https://github.com/sagiegurari/cargo-make
[mu-rust-pi-crate]: https://crates.io/crates/mu_pi
[mu-rust-build]: https://microsoft.github.io/mu/CodeDevelopment/rust_build/
[patina-repo]: https://github.com/OpenDevicePartnership/patina
[patina-plugfest-pdf]:
  https://uefi.org/sites/default/files/resources/5.%20Patina%20UEFI%20Rust_Microsoft_Kubacki_FINAL.pdf
[patina-plugfest-video]: https://www.youtube.com/watch?v=iKzHrhpCgUI
[patina-brighttalk]: https://www.brighttalk.com/webcast/18206/669463
[patina-brighttalk-video]: https://www.youtube.com/watch?v=KpWeqlWUj2s
[patina-doc]: https://opendevicepartnership.github.io/patina/patina.html
[patina-ecosystem]: https://opendevicepartnership.github.io/patina/background/patina_in_the_rust_ecosystem.html
[patina-memory-safety]: https://opendevicepartnership.github.io/patina/background/memory_safety_strategy.html
[patina-case-studies]: https://opendevicepartnership.github.io/patina/background/uefi_memory_safety_case_studies.html
[android-2021]: https://security.googleblog.com/2021/04/rust-in-android-platform.html
[android-2022]: https://security.googleblog.com/2022/12/memory-safe-languages-in-android-13.html
[android-2025]: https://blog.google/security/rust-in-android-move-fast-fix-things/
