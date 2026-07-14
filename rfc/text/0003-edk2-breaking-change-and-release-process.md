# RFC: Breaking Change and Release Process for EDK II

## Metadata

- **RFC Number**: 0003
- **Title**: Breaking Change and Release Process for EDK II
- **Status**: Accepted

## Change Log

- 2026-04-29: Initial RFC created.
  - This RFC is in the early stages of development and feedback is welcome on all aspects of the proposal.
- 2026-05-07:
  - Added information about using `PACKAGES_PATH` as a strategy to avoid breaking changes in the
    [Strategies to Avoid Breaking Changes](#strategies-to-avoid-breaking-changes) section.
  - Updated the Build System Breaking Change taxonomy to have more context about CI being a part of the category.
- 2026-05-12:
  - Added a new "Breaking Conditions" subsection to the [Breaking Change Taxonomy](#breaking-change-taxonomy) section to
    clarify how to communicate when a breaking change only impacts certain platforms or configurations.
- 2026-05-15:
  - Refactored breaking change documentation to be maintained in a `BREAKING-CHANGES.md` file maintained in the
  repository. The content previously required in the GitHub Tracking Issue now is included in a per-change entry in
  `BREAKING-CHANGES.md` that is added or updated in the same PR that introduces the breaking change.
- 2026-06-18:
  - Minor refactor of existing content with no changes to process. Typo and grammatical updates.
- 2026-06-30:
  - Separated removal and non-removal breaking change handling into a dedicated
    [Non-Removal Breaking Changes](#non-removal-breaking-changes) subsection to reduce the effort required to parse
    the differences from the [Deprecation Timeline](#deprecation-timeline) section.
  - Clarified the stable tag timeline numbering so `N` consistently refers to the deprecation active period and added
    a timeline diagram.
  - Added a "Library class dependency addition" subcategory to the taxonomy and split it into the single-instance and
    platform-implemented cases.
  - Added per-category guidance for structuring the two-PR (coexist, then remove) transition for removal changes.
  - Clarified that stable tag breaking change release notes are published to the GitHub Release for the stable tag.
  - Consolidated the duplicated breaking change state descriptions into a single
    [Breaking Change States](#breaking-change-states) table and removed duplicated GitHub issue content requirements.
  - Standardized role terminology (release owner, package maintainer) and added a
    [Terminology](#terminology) subsection defining the timeline terms.
  - Renamed the "Breaking Conditions" subsection to "Conditional Breaking Changes" for clarity.
  - Fixed typos and copy errors throughout.
- 2026-07-01:
  - RFC approved to merge in the July 1, 2026 TianoCore Stewards meeting.
- 2026-07-14:
  - Change the label `breaking-change` to `impact:breaking-change` to align with the existing label name used in the
    edk2 repository.

## Motivation

> Note: This RFC will use "EDK II" and "edk2" interchangeably to refer to the EDK II project and codebase. In most
> cases, "EDK II" will be used in the narrative and "edk2" will be used when referring to the GitHub repository
> itself or code artifacts within it.

EDK II is consumed by a large ecosystem of platform firmware implementations, both open-source (edk2-platforms) and
proprietary closed source. EDK II has a defined release process that ultimately results in code being tagged as
"stable" in the edk2 repository on a quarterly basis as managed in the [EDK II Stable Tag Planning](https://www.tianocore.org/tianocore-wiki.github.io/releases-history/planning-roadmaps/edk_ii_release_planning.html)
page on the [TianoCore Wiki](https://www.tianocore.org/tianocore-wiki.github.io/).

Today's release process includes:

- A [soft feature freeze](https://www.tianocore.org/tianocore-wiki.github.io/governance/project-management/soft_feature_freeze.html)
- A [hard feature freeze](https://www.tianocore.org/tianocore-wiki.github.io/governance/project-management/hard_feature_freeze.html)

  ```text
      Development Period         Soft Freeze        Hard Freeze     Stable Tag
  |-------- open ------------|---- limited ----|---- fixes only ----|
  ```

This RFC proposes augmenting not replacing most elements of the existing release process to more rigidly define how
breaking changes are handled in relation to the release process and how notable code changes targeting releases,
including breaking changes, can be better tracked by the community.

Today, the project lacks a formal contract that defines what constitutes a breaking change, how breaking changes are
communicated, and what obligations edk2 developers have toward platform consumers when introducing them. This results in
breaking changes being handled inconsistently:

- Some changes include migration guidance in their PR description and others do not.
- Platform owners often discover breaking changes only when their builds fail.
- There is no agreed-upon deprecation timeline for APIs, library classes, or PCDs. These are negotiated on a per-change
  basis, if at all.
- Behavioral changes that do not cause build failures can still silently alter firmware behavior in a way that is not
  communicated effectively to platform owners.
- Breaking changes are sometimes accompanied by edk2-platforms companion PRs, but these are not strictly required by
  enforced policy.
- There is no version-controlled record of breaking changes that platform consumers can consult or that reviewers can
  use to enforce documentation requirements during PR review.

This RFC proposes a breaking change policy that establishes a clear contract between edk2 developers and platform
consumers. The goal is to make breaking changes predictable, well-communicated, and manageable.

A central element of the proposed policy is a `BREAKING-CHANGES.md` file maintained in the edk2 repository. Each
breaking change is documented as an entry in this file, added or updated in the same PR that introduces the change.
This approach keeps breaking change documentation co-located with the source code, brings review and enforcement of
that documentation into the scope of normal PR review, and reduces reliance on tooling that scrapes content out of
GitHub issues to assemble release notes.

## Technology Background

The EDK II project is mostly developer driven, composed of a large and distributed community of contributors. In
addition, the project lacks an active project manager to coordinate and drive a detailed project planning schedule.

In the past, we have experimented with [GitHub projects](https://docs.github.com/issues/planning-and-tracking-with-projects/learning-about-projects/about-projects)
(e.g. [edk2 projects](https://github.com/tianocore/edk2/projects?query=is%3Aopen)) to track in-flight and planned work,
but developers tended to work outside the project infrastructure and an authority was not in place to enforce its use.

Based on that experience, this RFC does not propose using GitHub projects as part of the release process at this time.
Instead, a lighter-weight approach using [GitHub milestones](https://docs.github.com/issues/using-labels-and-milestones-to-track-work/about-milestones)
is proposed.

A GitHub milestone serves as a central point for tracking all work related to a specific release. In this case, a
milestone would be created for each EDK II stable tag to replace the [Release Planning](https://www.tianocore.org/tianocore-wiki.github.io/releases-history/planning-roadmaps/edk_ii_release_planning.html)
wiki page. In doing so, the milestone:

- Provides a formal way to specify the due date and serve as a record of stable tag activity after the release
- Provides a single source of truth for all work related to a release, including issues and pull requests
- Is defined once in the code repository where the work is happening and the stable tag is created instead of a separate
  wiki page
- Is automatically updated based on GitHub activity so milestone status is always up to date without manual effort

GitHub reports progress toward the planned milestone work (open issues and PRs) as a percentage, making it easy for the
community to assess readiness for an upcoming stable tag.

## Terminology

For consistency, this RFC uses the following roles and timeline terms throughout the document.

**Roles**:

- **Release owner**: The individual responsible for driving a stable tag release, including creating milestones,
  assembling release notes, and confirming process compliance for that release.
- **Package maintainer**: A maintainer or reviewer responsible for one or more edk2 packages, as recorded in the
  `Maintainers.txt` file, who approves changes to those packages.

**Timeline terms**:

- **Removal change**: A breaking change that removes or replaces existing functionality and therefore must follow the
  deprecation timeline.
- **Non-removal change**: A breaking change that only adds to or alters existing functionality without removing it (for
  example, adding a new library class dependency). It follows the communication requirements but not the deprecation
  timeline.
- **Deprecation timeline**: The sequence of stable tags across which a removal change is announced, deprecated, and
  finally removed.
- **Deprecation active period**: The period, beginning at the stable tag in which the deprecation PR is merged, during
  which the old code remains functional while platforms migrate.
- **Compatibility window**: The span of time during the deprecation active period in which both the old and new code
  coexist so platforms can migrate incrementally.
- **Deprecation conclusion point**: The earliest stable tag at which the deprecated item may be removed.

## Goals

1. **Classify breaking changes**: Define a taxonomy of breaking change types so developers and consumers share a
   common vocabulary.
   - See the [Breaking Change Taxonomy](#breaking-change-taxonomy) section for details.
2. **Establish a deprecation timeline**: Tie deprecation and removal to existing freeze periods and stable tag releases
   so platform consumers have predictable integration points and timelines for breaking changes.
   - See the [Deprecation Timeline](#deprecation-timeline) section for details.
3. **Formalize breaking change communication**: Ensure every breaking change includes actionable instructions and
   consistent information about breaking changes to help platform owners understand and adopt changes.
   - See the [BREAKING-CHANGES.md File](#breaking-changesmd-file) section for details on the required content of
     each entry, including migration guidance.
4. **Define GitHub process for tracking and communicating breaking changes**: Define a GitHub-based process using
   issues, labels, and milestones to track breaking changes and tie them to specific stable tag releases, making it
   easier for developers to manage breaking change process requirements in their normal interactions with the
   repository. Detailed breaking change documentation is maintained in `BREAKING-CHANGES.md` in the edk2 repository
   and reviewed as part of normal PR review, which directly feeds the breaking change section of stable tag release
   notes without requiring content to be scraped from GitHub issues.
   - See the [Requirements](#requirements) section for details on the GitHub process requirements.
5. **Coordinate cross-repository changes**: Define when and how edk2-platforms companion PRs are expected.
   - See the [Cross-Repository Coordination: edk2-platforms](#cross-repository-coordination-edk2-platforms) section for
     details.
6. **Provide avoidance strategies**: Document patterns that can reduce unnecessary breaking changes.
   - See the [Strategies to Avoid Breaking Changes](#strategies-to-avoid-breaking-changes) section for details.

At this time, it is not a goal of this RFC to specify changes to source code annotations or develop new tooling to track
and report breaking changes. This is considered an important topic area but it is expected to supplement the process
defined in this RFC. See [Unresolved Questions](#unresolved-questions) for details.

## Requirements

The requirements below reference concepts that are defined in detail later in the
[Implementation Design](#implementation-design) section, including the [Breaking Change Taxonomy](#breaking-change-taxonomy),
the [Deprecation Timeline](#deprecation-timeline), the [Breaking Change States](#breaking-change-states), and the
[BREAKING-CHANGES.md File](#breaking-changesmd-file). Readers new to the process may wish to read those sections first.

1. The EDK II release owner is responsible for creating GitHub milestones for each stable tag release. At any given
   time, there should be a milestone for at least the next three upcoming stable tag releases.
2. All breaking changes must be classified as source-level, behavioral, or build-system breaking changes, following the
   [Breaking Change Taxonomy](#breaking-change-taxonomy).
3. The breaking change must be classified as a removal or non-removal change (see the [Deprecation Timeline](#deprecation-timeline)
   and [Non-Removal Breaking Changes](#non-removal-breaking-changes) sections).
4. Every breaking change PR must add or update a corresponding entry in the `BREAKING-CHANGES.md` file maintained at
   the root of the edk2 repository.
   1. The entry must include the required content defined in the [BREAKING-CHANGES.md File](#breaking-changesmd-file)
      section.
   2. Reviewers must verify the `BREAKING-CHANGES.md` entry as part of normal PR review, including completeness of
      the migration guidance and correct categorization.
5. **Removal breaking changes**:
   1. Must follow a minimum deprecation timeline.
      - Note that this timeline is influenced by freeze periods as defined in the [Breaking Changes and Freeze Periods](#breaking-changes-and-freeze-periods)
      section.
   2. Must be tracked against three GitHub issues: a Tracking Issue, a Deprecation Issue, and a Removal Issue.
6. **Non-removal breaking changes** must still follow communication requirements (including the `BREAKING-CHANGES.md`
   entry) but do not need to follow the deprecation timeline or the three-issue creation requirements. A single
   Tracking Issue is required.
7. GitHub issues must follow the detailed requirements specified in section [Deprecation Timeline](#deprecation-timeline)
   and the required content defined in the [GitHub Issue Templates](#github-issue-templates) section. The Tracking
   Issue contains a high-level description of the breaking change and links to the corresponding entry in
   `BREAKING-CHANGES.md`. The Deprecation and Removal Issues contain only special notes about that part of the
   process when applicable.
8. Stable tag release notes must list breaking changes by category and subcategory to help platform owners
   understand the nature of the break and the appropriate mitigation steps.
   1. The breaking changes section of the stable tag release notes is derived directly from the entries in
      `BREAKING-CHANGES.md` that are associated with the stable tag's milestone (or that have transitioned state
      during the milestone window).
   2. For each breaking change, stable tag release notes must indicate what state the breaking change is in based on
      its open GitHub issues. The states, their issue combinations, and the corresponding release note phrasing are
      defined in the [Breaking Change States](#breaking-change-states) section, which is the single source of truth
      for these descriptions.

## UEFI/PI Specification Impact

This RFC establishes a process policy and does not directly modify UEFI or PI specification behavior. However,
breaking changes in edk2 may arise from specification updates through the Code First Process. In those cases:

- The breaking change classification and migration guidance requirements defined in this RFC still apply.
- Specification-driven breaking changes follow the same deprecation timeline unless the specification mandates an
  immediate change.

## Backward Compatibility

This RFC itself adds onto, as opposed to replacing, existing policies. The policies introduced are designed to improve
breaking change integration in the future as the result of defined processes.

## Platform/Package Impact

This policy applies to all packages in the edk2 repository and to the GitHub process for managing the repository.

## Unresolved Questions

- Should the project adopt a new `deprecated` annotation convention for source code?
  - Should additional tooling be developed to parse and report deprecated content to platform developers?
    - For example, in the "compatibility window" of a breaking change any platforms that pick up the code could have a
      build plugin list all deprecated code being used by the platform build to raise awareness to move to the new
      code.
  - Today this is handled in various ways such as:
    - Custom macros like [`ENABLE_MD5_DEPRECATED_INTERFACES`](https://github.com/tianocore/edk2/blob/master/CryptoPkg/Include/Library/BaseCryptLib.h#L89)
    - Project-standardized but seldom-used macros like [`DISABLE_NEW_DEPRECATED_INTERFACES`](https://github.com/tianocore/edk2/commit/72a8abde4009a4b933a2ea995cc0ad20ea01a5bf)
  - Compiler mechanisms exist that might be useful like:
    - The `deprecated` attribute in C/C++ ([gcc](https://gcc.gnu.org/onlinedocs/gcc-15.2.0/gcc/Common-Function-Attributes.html)/[msvc](https://learn.microsoft.com/cpp/cpp/deprecated-cpp?view=msvc-170))
    can be used to generate warnings when deprecated APIs are used. For example:

      ```c
      // GCC/Clang
      __attribute__((deprecated("Use StrCpyS instead. Removal in edk2-stable202702.")))
      CHAR16 *
      StrCpy (
        OUT CHAR16        *Destination,
        IN  CONST CHAR16  *Source
        );
      ```

    - `#pragma message` ([gcc](https://gcc.gnu.org/onlinedocs/gcc/Diagnostic-Pragmas.html#index-pragma_002c-diagnostic-1)/[msvc](https://learn.microsoft.com/cpp/preprocessor/message?view=msvc-170))
      exist as considerations but may not be obvious in build output.
- Should the project maintain a machine-readable deprecation "registry" (e.g., a YAML/JSON file tracking deprecated
  items and their planned removal stable tag)?
- Should a GitHub team be created for platform maintainers to join to receive notifications about breaking changes?
  - This allows the team to be mentioned in issues and pull requests in addition to being added as a reviewer to
    breaking change PRs.

## Prior Art/Related Work

### Rust Language

Rust introduced [Stability as a Deliverable](https://blog.rust-lang.org/2014/10/30/Stability/) in 2014 to
"provide stability without stagnation". This uses a channel-based approach to have reliable releases where users can
choose when to adopt new features and breaking changes by opting into stable, beta, or nightly channels. New features
and new APIs will be flagged as unstable (using Rust feature gates and stability attributes). Unstable features and
standard library APIs will only be available on the nightly branch, and only if you explicitly "opt in" to the
instability.

Rust also uses [editions](https://doc.rust-lang.org/edition-guide/editions/) to manage backward compatibility when
modifying the language.

The beta and stable releases only include features and APIs deemed stable, which represents a commitment to avoid
breaking code that uses those features or APIs.

> Since editions are opt-in, existing crates won’t use the changes unless they explicitly migrate into the new edition.
> For example, the latest version of Rust doesn’t treat async as a keyword unless edition 2018 or later is chosen.
>
> ...
>
> When creating editions, there is one most consequential rule: crates in one edition must seamlessly interoperate with
> those compiled with other editions. In other words, each crate can decide when to migrate to a new edition
>independently. This decision is ‘private’ - it won’t affect other crates in the ecosystem.

Tooling is provided (`cargo fix`) to automatically migrate code to new editions. The documentation states this is not
perfect, the automated process avoids changes to semantics and may require manual intervention in some cases.

### LLVM/Clang

LLVM has a [breaking change policy](https://llvm.org/docs/DeveloperPolicy.html#breaking) that requires the following:

- Adding a "vendors" GitHub team to the review for their awareness.
  - An expectation is that notified vendors can give early testing feedback on the changes before they are merged.
  - The GitHub teams are managed as:
    - [libc++ vendors](https://github.com/orgs/llvm/teams/libcxx-vendors)
    - [Clang vendors](https://github.com/orgs/llvm/teams/clang-vendors)
    - Those interested can join via the "Join team" button on the team page.
- An update to the *Potentially Breaking Changes* section of the release notes to call out the breaking change and
  provide migration guidance. This includes:

  > information about what the change is, what is potentially disruptive about it, as well as any code examples, links,
  > and motivation that is appropriate to share with users.
- After the change is merged:
  - The potentially disruptive changes should be posted in the "Announcements" channel on Discourse with a
    `potentially-breaking` label.
    - This is considered a lower-traffic alternative to joining the "vendors" GitHub team.

### Python

Python's [deprecation policy](https://peps.python.org/pep-0387/):

- Strives for a large benefit to breakage ratio where the incompatibility should be easy to resolve in affected code.
- Requires that a deprecation period extend through two release cycles where Python has a yearly release cadence.
- Requires that a feature cannot be removed without notice between any two consecutive releases.
- Requires that changes that are unable to raise a deprecation warning be consulted with the Python Steering Council to
  determine an appropriate course of action.
- Allows exceptions to the process by the Python Steering Council in "extreme situations" such as dangerously broken
  or insecure features or in cases where it is not possible for the feature to be used by any existing code.

There is a concept of "soft deprecation" where an API should no longer be used to write new code, but it remains safe to
continue using in existing code. The API remains documented and tested, but will not be developed further. A soft
deprecation is only mentioned in documentation.

In contrast, a "hard deprecation" is when an API is marked for removal and issues a `DeprecationWarning` at runtime.

Further details are in: [Making Incompatible Changes](https://peps.python.org/pep-0387/#making-incompatible-changes)

## Alternatives

### Alternative 1: No Formal Policy (Status Quo)

- **Pros**: No process overhead. Individual maintainers retain full discretion.
- **Cons**: Unpredictable for platform consumers. Inconsistent migration guidance. Breaks discovered late.
- **Why not chosen**: The ecosystem needs a more predictable contract between core and platform code.

### Alternative 2: Strict Stability Guarantee (No Breaking Changes)

- **Pros**: Maximum predictability for consumers. No source-level breaks between stable tags.
- **Cons**: Severely constrains code development and increases maintenance. May require extensive shim/compatibility
  layers.
- **Why not chosen**: Too rigid for a project that must track evolving specifications and hardware requirements.

### Alternative 3: Allow All Breaking Changes Outside of Freeze Periods

- **Pros**: For developers this is easy to follow and gives flexibility to focus on code changes rather than process.
- **Cons**: For consumers picking up changes from `master` in between stable tags, this is unpredictable and can lead
  to a much higher rate of platform breakage.
- **Why not chosen**: Based on historical feedback, this approach is too disruptive for platform consumers.

### Alternative 4: A Tiered Release Process

- **Pros**: Allows breaking changes to be made with less friction. Provides consumers with options for stability.
- **Cons**: Requires significant process overhead to manage multiple release channels. May fragment the ecosystem based
  on channels chosen, may delay adoption of important changes, and may cause confusion about which channel to use.
- **Why not chosen**: Not practical for the rate of change in edk2 which is lower than projects typically using release
  channels and the level of investment possible to maintain and explain multiple channels.

## Implementation Design

### Breaking Change Taxonomy

Breaking changes in edk2 often fall into three categories: **source-level**, **behavioral**, and **build-system**.

Each category has distinct subcategories. This section defines the taxonomy and provides examples of each type of
breaking change for clarity.

At this time, this taxonomy is primarily intended to guide communication and classification of breaking changes. The
stable tag release notes must list breaking changes by category and subcategory to help platform owners understand
the nature of the break and the appropriate mitigation steps.

This is important because a platform owner that encounters a build failure can quickly consult the "source-level"
section of the release notes to understand what changed and how to fix their code. On the other hand, a platform owner
may expect that if the incoming code compiles successfully, then there are no breaking changes. However, behavioral
changes can still break platforms without causing build failures. The "behavioral breaking changes" section of the
release notes is intended to provide a focused summary of such changes.

#### Category 1: Source-Level Breaking Changes

Source-level breaking changes cause compilation or link failures in downstream code.

| Subcategory                             | Description                                                                             | Example                                                |
|-----------------------------------------|-----------------------------------------------------------------------------------------|--------------------------------------------------------|
| **API signature change**                | Function prototype modification (parameters added/removed/retyped, return type changed) | `AllocatePool` gaining an alignment parameter          |
| **API removal**                         | Function, macro, or type definition removed from a public header                        | Removing `StrCpy` in favor of `StrCpyS`                |
| **Library class removal**               | An entire library class removed from a package DEC file                                 | Removing `PciExpressLib` library class                 |
| **Library class restructure**           | Library class split, merged, or moved between packages                                  | Moving a library class from `MdeModulePkg` to `MdePkg` |
| **Library class dependency addition**   | A module gains a new required library class dependency that platforms must resolve       | `DxeCore` adding a dependency on `MemoryBinLib`        |
| **GUID/Protocol/PPI rename or removal** | A published GUID, protocol, or PPI is renamed or removed                                | Renaming `gEfiSomethingProtocolGuid`                   |
| **PCD removal or type change**          | A PCD is removed, renamed, or has its datum type changed                                | Changing a `PcdsFixedAtBuild` UINT32 PCD to UINT64     |
| **Header file relocation**              | A public header moved to a different include path                                       | Moving `<Library/SomeLib.h>` to a different directory  |
| **Structure/enum layout change**        | Fields added, removed, reordered, or resized in a public structure or enum              | Adding a field to `EFI_DRIVER_BINDING_PROTOCOL` data   |

#### Category 2: Behavioral Breaking Changes

Behavioral breaking changes do not cause build failures but alter runtime behavior in ways that may break platform
assumptions.

| Subcategory                   | Description                                                                                  | Example                                                                           |
|-------------------------------|----------------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------|
| **Stricter input validation** | A function that previously accepted certain inputs now rejects them                          | A UEFI variable function rejecting attribute combinations it previously allowed   |
| **Changed default behavior**  | Default values, policies, or feature flags change                                            | A PCD default changing from `FALSE` to `TRUE`                                     |
| **Error handling change**     | A function returns different error codes or now returns errors where it previously succeeded | A protocol returning `EFI_UNSUPPORTED` where it previously returned `EFI_SUCCESS` |
| **Ordering/timing change**    | The order of operations, events, or callbacks changes                                        | DXE driver dispatch order changing due to dependency expression updates           |
| **Security hardening**        | Enforcement of previously unenforced security policies                                       | Enabling NX memory protection by default in DXE core                              |

#### Category 3: Build-System Breaking Changes

Build-system and continuous integration (CI) related breaking changes affect tooling, build configuration, or the
build process. The edk2 repository uses the same infrastructure (Stuart) for local and server CI. Changes to this
infrastructure or dependencies in the repository used by the infrastructure could break downstream platforms using
those same tools or dependencies in their local development or CI environments.

| Subcategory                   | Description                                                                                             | Example                                               |
|-------------------------------|---------------------------------------------------------------------------------------------------------|-------------------------------------------------------|
| **Tool version requirement**  | Minimum required version of a compiler, assembler, or build tool increases                              | Requiring GCC 12+ when GCC 5 was previously supported |
| **DSC/INF/DEC syntax change** | Build description file syntax or semantics changes                                                      | New required INF section or changed section behavior  |
| **Build flag change**         | Default or required compiler/linker flags change                                                        | Enabling `-Werror` for a previously warning-only flag |
| **Submodule update**          | A git submodule (e.g., `CryptoPkg/Library/OpensslLib/openssl`) is updated to a version with API changes | OpenSSL major version bump                            |
| **Python/tool dependency**    | Build-time Python package or tool dependency added or updated                                           | Requiring a new pip package in `pip-requirements.txt` |
| **BaseTools change**          | Modification to BaseTools that changes build behavior                                                   | `GenFds` output format change                         |

### Conditional Breaking Changes

In some cases, a breaking change may only impact certain platforms or configurations. For example, a change may only
cause build failures when building for a specific architecture or when a specific PCD is enabled. In such cases, the
description in the release notes should clearly indicate the conditions under which the break occurs to help platform
owners understand if they are affected and need to take action.

This does not change other requirements such as the need to follow the deprecation process.

### Non-Removal Breaking Changes

Breaking changes that involve **addition** or alteration rather than removal (for example, adding a new library class
dependency to an existing INF file) do not require deprecation but must still follow the communication requirements
defined in this RFC. For these changes:

- Only a single GitHub Tracking Issue is required. No Deprecation Issue or Removal Issue is created.
- The Tracking Issue may be closed by the single PR that makes the change.
- The PR **must** add the corresponding entry to `BREAKING-CHANGES.md`, and reviewers **must** verify it as part of
  normal PR review.
- The PR **must** have an `impact:breaking-change` label and link the Tracking Issue from the PR description.

Apply the requirements in the [BREAKING-CHANGES.md File](#breaking-changesmd-file) and
[GitHub Issue Templates](#github-issue-templates) sections, ignoring the deprecation-specific requirements (the
Deprecation Issue, the Removal Issue, and the [Deprecation Timeline](#deprecation-timeline)).

#### Library Class Dependency Additions

A common non-removal breaking change is adding a new required library class dependency to an existing module (for
example, `DxeCore` gaining a dependency on `MemoryBinLib`). Platforms that build the affected module must resolve the
new library class in their DSC or the build fails with an unresolved library class. These changes fall into two cases
that differ in the effort required of platform owners, and the `BREAKING-CHANGES.md` entry **must** state which case
applies.

- **Single expected instance**: The new library class has a single canonical instance provided in-tree that satisfies
  every consumer (for example, a `NULL` library instance or a single implementation owned by the same package). The
  migration is clear: platforms add a library class mapping in their DSC that points to the provided instance.
- **Platform-implemented**: The new library class defines an interface that platforms are expected to implement or
  select among multiple instances (for example, board-specific behavior with no single correct instance). This has a
  higher integration cost. The migration guidance **must** describe the interface contract, list any in-tree instances
  that may be reused, and explain what a platform must decide or implement.

In both cases, prefer providing a default in-tree instance where practical (see
[Compatibility Wrappers](#3-compatibility-wrappers) and [Additive APIs](#1-additive-apis)) so that the majority of
platforms fall into the lower-cost single-instance case.

### Deprecation Timeline

Source-level breaking changes that involve **removal** of existing functionality must follow a deprecation timeline
tied to stable tag releases. Breaking changes that do not involve removal follow the simpler process described in the
[Non-Removal Breaking Changes](#non-removal-breaking-changes) section instead.

#### Timeline Overview

Throughout this RFC, the stable tag numbering is anchored on the **deprecation active period**. `N` always refers to
the stable tag in which the deprecation active period begins (the stable tag whose development period the deprecation
PR is merged into).

- **Announced (pre-`N`)**: The breaking change is documented and approved, but the PR that begins the deprecation
  active period has not yet merged. No compatibility window is open yet.
- **Stable Tag `N` (deprecation active period)**: The deprecation PR is merged. The old and new code coexist and the
  compatibility window is open so platforms can migrate.
- **Stable Tag `N+1` (deprecation conclusion point)**: The earliest stable tag at which the deprecated item may be
  removed.

```text
  Announced (pre-N)        Stable Tag N                  Stable Tag N+1
  documented / approved    Deprecation Active            Deprecation Conclusion
|---------------------- |-- old + new coexist -------- |-- old removed -------->
                           (compatibility window)         (earliest removal)
```

#### Announcement (Pre-N)

The three GitHub issues (Tracking, Deprecation, and Removal) and the `BREAKING-CHANGES.md` entry are created when a
removal breaking change is first proposed and approved. This may happen before the PR that begins the deprecation
active period merges. When they are created ahead of that PR:

- All three issues are open and the `BREAKING-CHANGES.md` entry has status `Announced`.
- This corresponds to the **Announced** state in the [Breaking Change States](#breaking-change-states) section, giving
  platform owners early notice before any code changes.

Creating the entry and issues at announcement is recommended for larger or higher-impact changes. A smaller change may
instead create them together with the deprecation PR, in which case the change moves directly to the **Deprecation
Active** state when that PR merges (see the next section).

#### Time of Change (Stable Tag N): Deprecation Active Period

This period called the "deprecation active period" begins a "compatibility window" during which the old code remains
functional while platforms migrate to the new code.

The `BREAKING-CHANGES.md` entry (created at announcement or, at the latest, in this PR) **must** be present and its
status set to `Deprecation Active` when this PR merges. The entry contains the detailed information about the change
(type, motivation, what replaces it, migration guidance, and removal target). See the
[BREAKING-CHANGES.md File](#breaking-changesmd-file) section for the required content and format.

- Three GitHub issues must exist for the change, created at announcement or, at the latest, together with this PR.
  Each issue's required description content is defined in the [GitHub Issue Templates](#github-issue-templates)
  section:
  1. The "Tracking Issue": Serves as the central GitHub-side reference for the breaking change.
      - This issue **must** have a `deprecation:tracking` GitHub label.
  2. The "Deprecation Issue": A subissue that tracks the deprecation of the item.
     - This issue **must** have a `deprecation:active` GitHub label.
  3. The "Removal Issue": A subissue that tracks the eventual removal of the item.
     - This issue **must** have a `deprecation:removal` GitHub label.
  - Note: These GitHub issues are created irrespective of any existing issues such as bug or feature request issues
    related to the change.
- The GitHub Deprecation Issue **must** be linked to the pull request so GitHub shows the relationship between the
  change and the deprecation tracking issue in the PR UI (in the "Development" section).
- The GitHub issues **must** be associated with their targeted milestones for completion:
  - The Deprecation Issue: The milestone when the deprecation PR is expected to be merged to `master` to begin the
    deprecation active period.
  - The Removal Issue: The milestone when the removal PR is expected to be merged to `master` to remove the deprecated
    item.
  - The Tracking Issue: The milestone when all of the work for the breaking change will be complete. This is often the
    same milestone used for the Removal Issue but can be later if there are additional follow-up tasks after removal
    that are still related to the breaking change.
- The PR that introduces the breaking change must have an `impact:breaking-change` label.
- The PR **must** explicitly link to the GitHub Tracking Issue in the "Integration Instructions" section of the PR
  description to ensure reviewers are aware of the deprecation tracking issue.
- The PR **must** add or update the `BREAKING-CHANGES.md` entry for this change. Reviewers **must** verify the entry
  as part of normal PR review.
- The deprecated item **must** remain functional.
- Note: The GitHub Deprecation Issue will automatically be closed after the PR is merged. The GitHub Tracking Issue and
  GitHub Removal Issue remain open until the item is removed in a future PR.

#### Stable Tag N+1: Deprecation Conclusion Point

This period called the "deprecation conclusion point" is the earliest point at which the deprecated item can be removed.
It is expected that most "compatibility windows" will close during this time, which is the development period for the
next stable tag.

- The deprecated item **must** be removed if applicable.
- The PR removing the item **must** be linked (via GitHub) to the GitHub Removal Issue previously opened.
- The PR removing the item **must** update the corresponding entry in `BREAKING-CHANGES.md` to reflect that removal
  has occurred (for example, by updating a status field in the entry as defined in the
  [BREAKING-CHANGES.md File](#breaking-changesmd-file) section).
- Note: The GitHub Removal Issue will automatically be closed after the PR is merged. The GitHub Tracking Issue must
  be closed by a package maintainer or the release owner if all work related to the deprecation and removal is
  confirmed to be completed.

#### Exceptions to the Deprecation Timeline

| Exception                 | Condition                                                                                                                                                                                  | Required Approval                                                |
|---------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------|
| **Security fix**          | Change follows the [TianoCore security advisory process](../../security/processes/reporting_security_issues.md) and the vulnerability requires immediate removal of insecure functionality | Security response team + affected package maintainer(s)          |
| **Specification mandate** | A UEFI/PI specification revision requires immediate compliance                                                                                                                             | Package maintainer(s) with reference to the specification change |
| **Dead code removal**     | Code with no known consumers (verified by maintainer with strong evidence from known downstream projects)                                                                                  | Package maintainer(s)                                            |

### Structuring the Two-PR Transition

Every removal change is delivered as at least two PRs: a first PR that begins the deprecation active period while the
old and new code coexist, and a later PR that removes the old code. The value of the deprecation timeline depends on
the first PR keeping platforms building and running while they migrate. This section gives, per taxonomy category, the
recommended way to structure that first (coexistence) PR, and calls out cases where coexistence is not practical.

For some changes there is no practical way to support the old and new behavior at the same time. Those changes should
be avoided where possible. When they are unavoidable, they must use the [Exceptions to the Deprecation
Timeline](#exceptions-to-the-deprecation-timeline) process rather than a coexistence PR.

#### Source-Level

| Subcategory                           | Recommended first (coexistence) PR                                                                                               | Coexistence feasible?             |
|---------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|-----------------------------------|
| **API signature change**              | Add a new function (or versioned variant) with the new signature and keep the old one delegating to it. Deprecate the old.       | Yes                               |
| **API removal**                       | Introduce the replacement API first. Keep the old symbol as a thin wrapper that delegates to the replacement.                    | Yes                               |
| **Library class removal**             | Keep the old library class as a compatibility wrapper that delegates to the new implementation.                                  | Usually                           |
| **Library class restructure**         | Leave a forwarding stub (header/instance) at the old location and, where possible, resolve via `PACKAGES_PATH`.                  | Usually                           |
| **Library class dependency addition** | Provide a default in-tree instance (and a `NULL` instance where meaningful) so most platforms only add a DSC mapping.            | Usually                           |
| **GUID/Protocol/PPI rename**          | Keep the old C identifier as an alias (`#define` old = new) with the GUID value unchanged. Deprecate the old identifier.         | Yes (rename); No (value change)   |
| **PCD removal or type change**        | Introduce the new PCD. During the window accept both, preferring the new. Deprecate the old.                                     | Usually                           |
| **Header file relocation**            | Leave a forwarding header at the old path that includes the new path. Deprecate the old path.                                    | Yes                               |
| **Structure/enum layout change**      | Use a versioned/extensible structure and append new fields guarded by a version or size field. Avoid reordering existing fields. | Only if additive; otherwise avoid |

#### Behavioral

For behavioral changes, the standard coexistence mechanism is a PCD (or equivalent policy switch) whose default
preserves the existing behavior in the first PR. A later change flips the default or removes the PCD with its own
communicated notice. However, PCDs present their own challenges for platform owners and package maintainers. Consult
with the package owner and other stakeholders to determine the best approach for each change.

#### Build-System

Build-system changes should, in the first PR, accept both the old and the new form and warn on the old if feasible and
a population of platforms is expected to be affected. The second PR can then remove the old form. Examples:

- **Tool version requirement**: Continue supporting the current minimum while validating the new one. Raise the
  minimum in a later PR.
- **DSC/INF/DEC syntax change**: Accept both the old and new syntax in BaseTools and warn on the old form. Remove the
  old form in a later PR.
- **Build flag change**: Introduce the new flag as opt-in. Make it the default in a later PR.
- **Submodule / Python / tool dependency update**: Where the update carries API changes, provide a shim or support
  both versions during the window. When that is not feasible, coordinate the change as a single break and document it.

### Breaking Change States

The state of a removal breaking change is derived from which of its GitHub issues are open or closed. Note that an
*open* Deprecation or Removal Issue means that step of the process is planned but not yet complete. The issue is
closed when the corresponding PR merges. This table defines the state descriptions used in both the
[Requirements](#requirements) and the [Stable Tag Release Notes Example](#stable-tag-release-notes-example).
`N` refers to the deprecation active period as defined in the [Timeline Overview](#timeline-overview).

| State                | Tracking Issue | Deprecation Issue | Removal Issue | Meaning / release note phrasing                                                                                                                              |
|----------------------|----------------|-------------------|---------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Announced**        | Open           | Open              | Open          | Breaking change announced. The deprecation active period has not begun; it will begin in a future stable tag, with removal one stable tag later or greater.  |
| **Deprecation Active** | Open         | Closed            | Open          | Deprecation active as of this release (`N`). Follow migration guidance in `BREAKING-CHANGES.md`. Removal planned for `N+1` or later.                         |
| **Removed**          | Open or Closed | Closed            | Closed        | Deprecated item removed in this release. Follow migration guidance in `BREAKING-CHANGES.md` to update to the new behavior.                                    |

The Tracking Issue may remain open in the **Removed** state if follow-up work related to the breaking change is still
outstanding. It is closed only when all subissues are closed and all related work is confirmed complete.

The following combinations are invalid and indicate the issues are out of sync with the actual state. The package
maintainer and release owner should investigate and correct the issues:

- Deprecation Issue open + Removal Issue closed (removal cannot complete before deprecation begins).
- Tracking Issue closed + any subissue open (the Tracking Issue may only close after all subissues close).

### BREAKING-CHANGES.md File

`BREAKING-CHANGES.md` is a markdown file maintained at the root of the edk2 repository. It serves as an in-tree record
of breaking changes and provides a reviewed and ready source of truth for the breaking change section of stable tag
release notes. Each breaking change has a single entry in this file. The entry is added in the PR that introduces the
breaking change and updated in any subsequent PRs that change its state (for example, when the deprecated item is
removed).

Initially, this breaking change process captured these details in GitHub issue descriptions, while this lent itself
well to GitHub automation to construct the release notes, it had a number of drawbacks versus simply maintaining the
information in `BREAKING-CHANGES.md`. These are the main reasons for maintaining breaking change information in a
checked in file:

- The documentation is version controlled alongside the code it describes.
- The documentation is reviewed in the same PR that introduces the breaking change, bringing review and enforcement
  of documentation requirements into the scope of normal PR review.
- No project overhead is needed to maintain expertise and knowledge about automation used during the release notes
  generation process.

#### File Structure

`BREAKING-CHANGES.md` is organized by stable tag milestone, with the most recent milestone first. Within each
milestone section, entries are grouped by category (Source-Level, Behavioral, Build-System) and then by subcategory
following the [Breaking Change Taxonomy](#breaking-change-taxonomy). Each entry is identified by a stable anchor so
it can be linked from GitHub issues, PR descriptions, and release notes.

A high-level outline:

```text
# EDK II Breaking Changes

## edk2-stable202702

### Source-Level Breaking Changes

#### Changes with Removal

<entry>
<entry>

#### Changes without Removal

<entry>

### Behavioral Breaking Changes

<entry>

### Build-System Breaking Changes

<entry>

## edk2-stable202611

...
```

#### Required Entry Content

Each entry in `BREAKING-CHANGES.md` uses a template. The required content is:

- A short title describing the change.
- A status field indicating the current state of the change. Allowed values:
  - `Announced`: The breaking change is announced; the deprecation active period has not yet begun.
  - `Deprecation Active`: The deprecation active period has begun; the compatibility window is open.
  - `Removed`: The deprecated item has been removed from the codebase.
- Links to the associated Tracking Issue, Deprecation Issue, and Removal Issue (Removal Issue is omitted for
  non-removal changes).
- Type of breaking change: Source-level (indicate removal or non-removal), Behavioral, or Build-system, including
  the subcategory from the [Breaking Change Taxonomy](#breaking-change-taxonomy).
- Why it Changed: The motivation or rationale for the breaking change.
- What replaces it (or why no replacement is needed).
- Migration guidance (see next section).
- Removal changes must additionally include:
  - What is deprecated: A clear description of what was modified, removed, or restructured.
  - The earliest stable tag at which removal may occur (may not be the upcoming stable tag).
- Breaking conditions, when the change only affects certain configurations (see [Conditional Breaking Changes](#conditional-breaking-changes)).
- Companion edk2-platforms PR link, when applicable.

##### Migration Guidance

Migration guidance must explain what platform owners need to do to update their code to work with the new change. The
guidance should be as specific and actionable as possible.

###### Example 1: API Signature Change

```text
### Breaking Change: AllocatePool gains an Alignment parameter in MemoryAllocationLib

**Status**: Deprecation Active
**Tracking Issue**: tianocore/edk2#XXXX
**Deprecation Issue**: tianocore/edk2#XXXX
**Removal Issue**: tianocore/edk2#XXXX
**Type**: Source-Level (Removal) - API signature change

**What changed**: `AllocatePool` in `MemoryAllocationLib` now takes an additional
`Alignment` parameter.

**Old signature**:
  VOID * AllocatePool (IN UINTN AllocationSize);

**New signature**:
  VOID * AllocatePool (IN UINTN AllocationSize, IN UINTN Alignment);

**Why it changed**: Several callers required aligned allocations and were
implementing alignment fix-ups locally. Centralizing alignment in the API
reduces duplicated code and aligns with the new memory protection model.

**What replaces it**: The same `AllocatePool` function with the new signature.

**How to migrate**: Add `0` as the second parameter to retain default alignment
behavior:
  Buffer = AllocatePool (Size, 0);

**Earliest removal**: edk2-stable202702
**Companion PR**: https://github.com/tianocore/edk2-platforms/pull/XXXX
```

###### Example 2: PCD Default Change

```text
### Breaking Change: PcdEnableNxMemoryProtection default changed to TRUE

**Status**: Deprecation Active
**Tracking Issue**: tianocore/edk2#XXXX
**Deprecation Issue**: tianocore/edk2#XXXX
**Removal Issue**: tianocore/edk2#XXXX
**Type**: Behavioral - Changed default behavior

**What changed**: `PcdEnableNxMemoryProtection` in `MdeModulePkg.dec` now defaults
to TRUE.

**Previous default**: FALSE
**New default**: TRUE

**Why it changed**: Aligns the default with the project's memory protection
direction and matches the value already used by the majority of in-tree
platforms.

**How to migrate**: If your platform requires the previous behavior, set the PCD
to FALSE in your platform DSC:
  gEfiMdeModulePkgTokenSpaceGuid.PcdEnableNxMemoryProtection|FALSE
```

#### Updating an Entry

When the state of a breaking change changes, the corresponding entry in `BREAKING-CHANGES.md` must be updated in the
PR that drives the state change. For example:

- The PR that removes a deprecated item must update the entry's `Status` to `Removed`.
- A PR that adjusts migration guidance based on community feedback must update the migration guidance text.

Entries are not deleted when a breaking change is complete. They remain in the file as a historical record of the
change and continue to be referenced from the corresponding stable tag release notes.

### GitHub Issue Templates

Each of the three GitHub issues created as part of the deprecation process (Tracking Issue, Deprecation Issue, and
Removal Issue) will have a corresponding GitHub issue template that defines required content for the issue description.

The GitHub issues exist primarily to provide GitHub-side process tracking (labels, milestones, subissue relationships,
PR linkage). The detailed breaking change documentation is managed in `BREAKING-CHANGES.md`. The issue descriptions in
the GitHub issues are intentionally minimal to prevent redundancy with the file.

The required content for each GitHub issue template is defined in this section. Additional content can be included at
the discretion of the issue creator but the required content must be present to ensure consistency.

#### Tracking Issue

The Tracking Issue serves as the central GitHub-side reference for a breaking change. The Deprecation Issue and
Removal Issue are subissues of the Tracking Issue.

Required content for the Tracking Issue template:

- A high-level summary of the breaking change (e.g. a few sentences).
- Any other relevant information that is not already captured in the `BREAKING-CHANGES.md` change.

All other detail (motivation, what replaces it, migration guidance, removal target, etc.) should only appear in the
`BREAKING-CHANGES.md` entry and not be duplicated in the Tracking Issue description.

#### Deprecation Issue

The Deprecation Issue tracks the beginning of the "deprecation active" period for the change. When the issue is open
the deprecation active period is planned but the PR to introduce the deprecation has not been merged yet. When the
issue is closed, the deprecation active period has begun and the "compatibility window" is now open for platforms to
migrate to the new code.

Required content for the Deprecation Issue template:

- Any special notes about the deprecation portion of the process (for example, details about how the affected item
  is marked as deprecated).
- If there are no special notes, the issue description must be: `Breaking change process issue.`.

#### Removal Issue

The Removal Issue tracks the eventual removal of the deprecated item. When the issue is open, the old item is still
present in the codebase and platforms should be using the migration guidance to move away from it. When the issue is
closed, the item has been removed from the codebase.

Required content for the Removal Issue template:

- Any special notes about the removal portion of the process.
- If there are no special notes, the issue description must be: `Breaking change process issue.`.

### Breaking Changes and Freeze Periods

**Minimum deprecation period requirement**: The PR initiating the "Deprecation Active" period must be merged to `master`
before the soft freeze period begins. If the PR is merged during the soft freeze or later, the deprecation period does
not begin until the next stable tag, effectively delaying removal by one additional stable tag.

| Period                         | Breaking Changes Allowed?                | Conditions                                                                                                                                                                                                                                   |
|--------------------------------|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Development period**         | Yes                                      | Must follow all requirements in this RFC                                                                                                                                                                                                     |
| **Soft freeze to hard freeze** | Only if previously reviewed and approved | The breaking change PR must have received maintainer approval before the soft freeze. If the change is merged, the "deprecation active" period begins after the stable tag.                                                                  |
| **Hard freeze to stable tag**  | No (with exceptions)                     | Only bug fixes and security fixes are accepted. A breaking change that is itself a bug fix or security fix may be merged with maintainer approval. If the change is merged, the "deprecation active" period begins after the stable tag.     |

### Cross-Repository Coordination: edk2-platforms

When a breaking change in edk2 affects code in edk2-platforms, the following process applies:

#### edk2-platforms PR Requirement

1. **Identification**: For any breaking change, the edk2 PR author must assess whether the change affects edk2-platforms
   by searching for usages of the affected APIs, PCDs, GUIDs, or build patterns in edk2-platforms.

2. **Companion PR submission**: If edk2-platforms is affected, a companion PR must be submitted to edk2-platforms
   that resolves the break. The companion PR should:
   - Reference the edk2 PR in its description
   - Be available concurrently with the edk2 PR

3. **Merge ordering**: The edk2 PR and edk2-platforms companion PR should be coordinated so that:
   - The edk2-platforms PR is ready to merge when the edk2 PR merges
   - The window during which edk2-platforms is broken against edk2 `master` is minimized

4. **Maintainer responsibility**: The edk2 package maintainer approving the breaking change is responsible for
   verifying that the companion PR exists (when applicable) before approving the edk2 PR.

### Stable Tag Release Notes Example

The [Requirements](#requirements) section of this RFC requires that stable tag release notes call out breaking changes
by category and subcategory to help platform owners understand the nature of the break and the appropriate mitigation
steps.

The breaking change section of stable tag release notes is built directly from the entries in `BREAKING-CHANGES.md`
that are associated with the stable tag's milestone (or that have transitioned state during the milestone window).
Because each entry already contains the type, status, migration guidance, and links to the associated GitHub issues,
the release owner can assemble the breaking change section by selecting the relevant entries from the file and,
where desired, copying or summarizing them into the release notes. GitHub milestones still provide the boundary that
defines which entries belong to a given stable tag.

The assembled release notes, including the breaking change section, are published in the GitHub Release for the stable
tag so the complete notes can be browsed across releases from the GitHub Releases page. `BREAKING-CHANGES.md` remains
the version-controlled source. The GitHub Release is the published, reader-facing view for a given stable tag.

#### High-Level Release Notes Example

This example only shows an outline of relevant sections of the release notes related to breaking changes. The actual
release notes would include additional sections such as new features, bug fixes, and other changes.

Each entry below is a summary derived from the corresponding entry in `BREAKING-CHANGES.md`. Each entry links to
both the in-tree entry and the GitHub Tracking Issue. The `State` phrasing for each entry is based on the
[Breaking Change States](#breaking-change-states) section, adapted as needed for the release notes.

```text
## Breaking Changes

For full details on each breaking change, including motivation, what replaces it, and migration guidance, see the
corresponding entry in `BREAKING-CHANGES.md` at the stable tag.

> Note: Links to entries in the below section refer to links to that section of the markdown file.

### Source-Level Breaking Changes

#### Changes with Removal

- <Link to BREAKING-CHANGES.md entry> (<Link to Tracking Issue>)
  - Type: API signature change with removal
  - State: Deprecation active, removal planned for edk2-stable202702 or later.
- <Link to BREAKING-CHANGES.md entry> (<Link to Tracking Issue>)
  - Type: Protocol removal
  - State: New breaking change introduced in this release, deprecation active, follow migration guidance in
    `BREAKING-CHANGES.md`, removal planned for edk2-stable202702 or later.

#### Changes without Removal

- <Link to BREAKING-CHANGES.md entry> (<Link to Tracking Issue>)
- <Link to BREAKING-CHANGES.md entry> (<Link to Tracking Issue>)

### Behavioral Breaking Changes

- <Link to BREAKING-CHANGES.md entry> (<Link to Tracking Issue>)
  - State: New breaking change introduced in this release, deprecation active, follow migration guidance in
    `BREAKING-CHANGES.md`, removal planned for edk2-stable202702 or later.
- <Link to BREAKING-CHANGES.md entry> (<Link to Tracking Issue>)
  - State: Deprecated code removed in this release, follow migration guidance in `BREAKING-CHANGES.md` to update
    your code to the new behavior.

### Build-System Breaking Changes

- <Link to BREAKING-CHANGES.md entry> (<Link to Tracking Issue>)
  - State: New breaking change introduced in this release, deprecation active, follow migration guidance in
    `BREAKING-CHANGES.md`, removal planned for edk2-stable202702 or later.
```

### Strategies to Avoid Breaking Changes

Developers should prefer non-breaking approaches when possible. The following strategies can reduce the frequency and
impact of breaking changes.

#### 1. Additive APIs

When adding functionality, consider whether adding new functions alongside existing ones is possible instead of
modifying existing APIs in a way that breaks callers.

```c
// This change adds an Alignment parameter that breaks all existing callers of AllocatePool:
VOID * AllocatePool (IN UINTN Size, IN UINTN Alignment);  // Breaks all callers

// This change adds a new function that allows callers to opt-in to the new behavior without breaking existing code:
VOID * AllocatePool (IN UINTN Size);                             // Unchanged
VOID * AllocateAlignedPool (IN UINTN Size, IN UINTN Alignment);  // New
```

#### 2. PCD-Controlled Behavior Changes

When adding a PCD, use a default value that preserves existing behavior to avoid breaking platforms that do not
explicitly set the PCD.

```ini
# In a package .dec file, a new PCD should have a backward-compatible default value
gEfiMdeModulePkgTokenSpaceGuid.PcdNewFeatureEnable|FALSE|BOOLEAN|0x00000XXX
```

This allows the behavior change to be introduced without breaking existing platforms, which retain the previous
default until they explicitly opt-in.

However, in most cases, PCDs should not solely be used for simply controlling a breaking change rollout. The PCD itself
becomes a part of the package configuration interface and its modification or future removal becomes a breaking change
in its own right.

#### 3. Compatibility Wrappers

When restructuring library classes, consider whether a thin compatibility wrapper in the old location that delegates to
the new implementation can minimize code overhead during the transition period (deprecation period) while still
allowing platforms to migrate incrementally.

#### 4. Preserve GUIDs for Binary Compatible Changes

When renaming a protocol, PPI, or named GUID, keep the GUID value unchanged if the interface itself is unchanged.
Only rename the C identifier. This avoids breaking code that references the GUID by value rather than by name.

#### 5. Create Interfaces That Can Scale

When a change is needed to support new functionality, consider designing the API to be extensible for future
changes in a way that minimizes the likelihood of future breaking changes. For example, using a version field in a
structure can allow new fields to be added to the end of the structure in the future without having to define a new
structure or change a GUID that might identify the structure definition.

#### 6. Using the Packages Path

The `PACKAGES_PATH` environment variable is used to describe the set of paths where packages can be found. This allows
paths to the packages to be relative to any path in `PACKAGES_PATH`. For example, if `PACKAGES_PATH` includes the edk2
and edk2-platforms repositories, then references to libraries in edk2 from code in edk2-platforms will resolve to the
corresponding package in edk2 regardless of the relative path between the two repositories. The packages path can be
used to avoid a package relocation from being a breaking change if the package is moved to a location already included
in `PACKAGES_PATH`.

## Testing Strategy

Since this RFC defines a process policy rather than a code change, testing will focus on validating that the process is
followed correctly when breaking changes are introduced. This will involve:

1. **Process compliance checks**: When a breaking change PR is merged, verify that the required GitHub issues are
   created with the correct labels and content, that the PR adds or updates an entry in `BREAKING-CHANGES.md` with
   the required content, that the PR description links to the Tracking Issue, and that any companion edk2-platforms
   PR is submitted as required.
2. **Release note validation**: For stable tag releases, review the release notes to ensure that all breaking changes
   present in `BREAKING-CHANGES.md` for the stable tag's milestone are reflected and properly categorized according to
   the taxonomy defined in this RFC.

## Migration/Adoption Plan

There are two major phases planned to breaking change policy:

1. **Process Definition**: Finalize the process policy as defined in this RFC, including any adjustments based on
   feedback from the community.
2. **Source Code Annotation and Tooling (Future Work)**: Based on the process defined in this RFC, evaluate the need for
   source code annotations (e.g., `deprecated` attributes) and tooling to track and report breaking changes. This may
   involve defining new conventions for marking deprecated code and developing tools to generate reports for platform
   developers about deprecated APIs they are using.

## Guide-Level Explanation

This section summarizes the process from each perspective as a quick checklist. The authoritative rules are in the
[Requirements](#requirements) and [Implementation Design](#implementation-design) sections.

**For Contributors**:

- Classify the change (see [Breaking Change Taxonomy](#breaking-change-taxonomy); removal vs non-removal).
- Add or update the [`BREAKING-CHANGES.md`](#breaking-changesmd-file) entry in the same PR.
- Create the required GitHub issues with the correct labels ([Deprecation Timeline](#deprecation-timeline) /
  [Non-Removal Breaking Changes](#non-removal-breaking-changes)) and link the Tracking Issue from the PR description.

**For Reviewers**:

- Verify the classification and that the [`BREAKING-CHANGES.md`](#breaking-changesmd-file) entry is complete
  (including migration guidance). Treat that entry as a first-class part of the review.
- Confirm the required GitHub issues, labels, milestones, and Tracking Issue link are present.

**For Maintainers**:

- Ensure all process requirements are met before approving, including edk2-platforms
  [coordination](#cross-repository-coordination-edk2-platforms) when applicable.
- Monitor deprecation/removal progress via the GitHub issues and ensure state transitions are reflected in
  [`BREAKING-CHANGES.md`](#breaking-changesmd-file) (see [Breaking Change States](#breaking-change-states)).

**For Platform Owners**:

- On each stable tag, review the release notes and follow the migration guidance in the referenced
  [`BREAKING-CHANGES.md`](#breaking-changesmd-file) entries.
- If integrating from `master` between stable tags, read `BREAKING-CHANGES.md` at the commit you are integrating to
  see the current set of breaking changes and their state.
