# EDK II Draft Specification

The documents in this page are the latest draft revisions using the
[Gitbook Markdown](https://github.com/GitbookIO/gitbook/blob/main/docs/syntax/markdown.md)
source format and published using the [Gitbook Action](https://github.com/marketplace/actions/gitbook-action).
The source content for these documents are in GIT repositories in the
[Tianocore-docs](https://github.com/tianocore-docs) organization hosted by [GitHub](https://github.com).
Document feedback, issues, and feature requests can be entered as a GitHub Issues in the document GitHub repository.

* **_EDK II Build Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-BuildSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-BuildSpecification/draft/edk2-BuildSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-BuildSpecification/draft/edk2-BuildSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-BuildSpecification/draft/edk2-BuildSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-BuildSpecification)
\] This document describes the EDK II Build Architecture. This specification was
designed to support new build requirements for building EDK II modules and EDK
components within the EDK II build infrastructure as well as to generate binary
firmware images and Unified Extensible Firmware Image (UEFI) applications.

* **_EDK II DEC Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-DecSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-DecSpecification/draft/edk2-DecSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-DecSpecification/draft/edk2-DecSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-DecSpecification/draft/edk2-DecSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-DecSpecification)
\] This document describes the EDK II Declaration (DEC) file format. This format
was designed to support building packaging and distribution of EDK II modules,
as well as for building platforms and modules using the EDK II build infrastructure.
EDK II declaration files may be created during installation of a distribution that
follows the UEFI Platform Initialization Distribution Package Specification. They
may also be created manually.  The EDK II Build Infrastructure supports generation
of UEFI 2.5 and PI 1.4 (Unified EFI, Inc.) compliant binary images.

* **_EDK II INF Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-InfSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-InfSpecification/draft/edk2-InfSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-InfSpecification/draft/edk2-InfSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-InfSpecification/draft/edk2-InfSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-InfSpecification)
\] This document describes the EDK II build information (INF) file format. This format
supports the new build requirements of build EDK components and EDK II modules within
the EDK II build infrastructure. The EDK II Build Infrastructure supports creation of
binary images that comply with Unified EFI (UEFI) 2.5 and UEFI Platform Infrastructure
(PI) 1.4 specifications.

* **_EDK II DSC Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-DscSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-DscSpecification/draft/edk2-DscSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-DscSpecification/draft/edk2-DscSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-DscSpecification/draft/edk2-DscSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-DscSpecification)
\] This document describes the EDK II Platform Description file (DSC) format. The EDK Build Tools
are included as part of the EDK II compatibility package. In order to use EDK II Modules or the
EDK II Build Tools, an EDK II DSC and FDF file must be used. EDK II tools use INI style text based
files to describe components, platforms and firmware volumes. While similar to EDK DSC files, the
EDK II DSC file format is different, and new utilities have been provided to process these files.
The EDK II Build Infrastructure supports creation of binary images that comply with Unified EFI
(UEFI) 2.5 and UEFI Platform Infrastructure (PI) 1.4 specifications.

* **_EDK II FDF Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-FdfSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-FdfSpecification/draft/edk2-FdfSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-FdfSpecification/draft/edk2-FdfSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-FdfSpecification/draft/edk2-FdfSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-FdfSpecification)
\] This document describes the EDK II Flash Description (FDF) file format. This format was
designed to support new build requirements of building EDK and EDK II modules within the
EDK II build infrastructure. The EDK II Build Infrastructure supports generation of current
Unified EFI, Inc. (UEFI 2.5 and PI 1.4) compliant binary images. The FDF file is used to
describe the content and layout of binary images. Binary images described in this file may
be any combination of boot images, capsule images or PCI Options ROMs.

* **_EDK II UNI Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-UniSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-UniSpecification/draft/edk2-UniSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-UniSpecification/draft/edk2-UniSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-UniSpecification/draft/edk2-UniSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-UniSpecification)
\] This document describes the Multi-String build information (UNI) file format . See details
in the Revision History in the document for more details.

* **_EDK II Image Definition IDF File Format Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-IdfSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-IdfSpecification/draft/edk2-IdfSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-IdfSpecification/draft/edk2-IdfSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-IdfSpecification/draft/edk2-IdfSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-IdfSpecification)
\] This document describes file format for Image Description files that are used
to create HII Image Packages introduced in the _Unified Extensible Firmware
Interface Specification_, Version 2.1.

* **_EDK II VFR Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-VfrSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-VfrSpecification/draft/edk2-VfrSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-VfrSpecification/draft/edk2-VfrSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-VfrSpecification/draft/edk2-VfrSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-VfrSpecification)
\] To simplify the creation of Internal Forms Representation (IFR), a high-level Visual Forms
Representation (VFR) language is described in this document. Using this language syntax, a
compiler can be designed to take an ordinary text file containing VFR as an input, and output
IFR for use in a user’s program. There are various methods to define the VFR language.

* **_EDK II Meta-Data Expression Syntax Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-MetaDataExpressionSyntaxSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-MetaDataExpressionSyntaxSpecification/draft/edk2-MetaDataExpressionSyntaxSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-MetaDataExpressionSyntaxSpecification/draft/edk2-MetaDataExpressionSyntaxSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-MetaDataExpressionSyntaxSpecification/draft/edk2-MetaDataExpressionSyntaxSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-MetaDataExpressionSyntaxSpecification)
\] This document describes the syntax of expression statements for EDK II Meta-data files
used in data fields, feature flag expressions and conditional directive statements.

* **_EDK II PCD Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-PcdSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-PcdSpecification/draft/edk2-PcdSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-PcdSpecification/draft/edk2-PcdSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-PcdSpecification/draft/edk2-PcdSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-PcdSpecification)
\] This document discusses the mechanisms and configuration entries required to make it
easy to write portable silicon modules and to port the Framework from platform to platform.

* **_EDK II C Coding Standards Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-CCodingStandardsSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-CCodingStandardsSpecification/draft/edk2-CCodingStandardsSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-CCodingStandardsSpecification/draft/edk2-CCodingStandardsSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-CCodingStandardsSpecification/draft/edk2-CCodingStandardsSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-CCodingStandardsSpecification)
\] The EDK II C Coding Standards Specification establishes a set of rules intended not as
a constraint, but as an enabling philosophy which will:
  * Establish uniformity of style.
  * Set minimum information content requirements.
  * Allow all programmers to easily understand the code.
  * Facilitate support tasks by establishing uniform conventions.
  * Ease documentation efforts by embedding the design documentation in the code.
  * Reduce customer and new employee learning curves by providing accurate code documentation and uniform style.

  These rules apply to all code developed.

* **_EDK II Python Development Process Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-PythonDevelopmentProcessSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-PythonDevelopmentProcessSpecification/draft/edk2-PythonDevelopmentProcessSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-PythonDevelopmentProcessSpecification/draft/edk2-PythonDevelopmentProcessSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-PythonDevelopmentProcessSpecification/draft/edk2-PythonDevelopmentProcessSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-PythonDevelopmentProcessSpecification)
\] The EDK II Python Development Process Specification defines a set of python
coding standards, development flow, and tools to help to identify and fix
deviations in written code. These standards, flow and tools to establish:
  * Uniformity of style
  * Uniform conventions
  * To maintain consistency
  * To maintain extensibility
  * To improve readability
  * To improve maintainability, reusability

  These rules apply to all code developed.

* **_EDK II Minimum Platform Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/edk2-MinimumPlatformSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/edk2-MinimumPlatformSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-MinimumPlatformSpecification/draft/edk2-MinimumPlatformSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-MinimumPlatformSpecification)
\] The EDK II Minimum Platform Specification describes the required and optional elements for an EDK II based platform
with the following objectives:
* Define a structure that enables developers to consistently navigate source code, execution flow, and the functional
results of bootstrapping a system.
* Enable a minimal platform where minimal is defined as the minimal firmware implementation required to produce a basic
solution that can be further extended to meet a multitude of computing segment needs.
* Establish definitions for common, silicon, platform, and board packages and guidelines for minimizing coupling between
these packages.
  * Enable large granularity binary solutions.

* **_EDK II Driver Writer's Guide for UEFI 2.3.1_** \[
[HTML](https://tianocore-docs.github.io/edk2-UefiDriverWritersGuide/draft/),
[PDF](https://tianocore-docs.github.io/edk2-UefiDriverWritersGuide/draft/edk2-UefiDriverWritersGuide-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-UefiDriverWritersGuide/draft/edk2-UefiDriverWritersGuide-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-UefiDriverWritersGuide/draft/edk2-UefiDriverWritersGuide-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-UefiDriverWritersGuide)
\] This document is designed to aid in the development of UEFI Drivers using the EDK II open source
project as a development environment.

* **_EDK II Module Writer's Guide_** \[
[HTML](https://tianocore-docs.github.io/edk2-ModuleWriteGuide/draft/),
[PDF](https://tianocore-docs.github.io/edk2-ModuleWriteGuide/draft/edk2-ModuleWriteGuide-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-ModuleWriteGuide/draft/edk2-ModuleWriteGuide-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-ModuleWriteGuide/draft/edk2-ModuleWriteGuide-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-ModuleWriteGuide)
\] This document is designed to aid in the development of EDK II  Modules using the EDK II open source
project as a development environment.

---

* **_EDK II Template Specification_** \[
[HTML](https://tianocore-docs.github.io/edk2-TemplateSpecification/draft/),
[PDF](https://tianocore-docs.github.io/edk2-TemplateSpecification/draft/edk2-TemplateSpecification-draft.pdf),
[MOBI](https://tianocore-docs.github.io/edk2-TemplateSpecification/draft/edk2-TemplateSpecification-draft.mobi),
[EPUB](https://tianocore-docs.github.io/edk2-TemplateSpecification/draft/edk2-TemplateSpecification-draft.epub),
[GitHub](https://github.com/tianocore-docs/edk2-TemplateSpecification)
\] This document is a template that can be copied to start a new Tianocore Gitbook document.
It also provides examples for styles and formats commonly found in Tianocore specifications.
