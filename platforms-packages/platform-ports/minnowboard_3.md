# MinnowBoard 3

[EDK II Platforms](edk_ii_platforms.md) | [Intel® Processor Platforms](edk_ii_platforms.md#intel-processor-platforms)

***

## Migration to [devel-IntelAtomProcessorE3900](intel_atom_processor_e3900.md)

Note to developers: Release 0.70 updates the codebase to use [UDK2018](../../releases-history/archives/udk2018.md), and
moves to a new branch of edk2-platforms on github ([devel-IntelAtomProcessorE3900](intel_atom_processor_e3900.md)). Releases
prior to 0.69, based on the devel-MinnowBoard3-UDK2017 & devel-MinnowBoard3 branches, are described below for historical
purposes.

## MinnowBoard 3 in edk2-platforms

This codebase is designed for the MinnowBoard 3 platform and Leaf Hill Customer Reference Board (CRB), using the [Intel
Atom® Processor E3900 Series](https://www.intel.com/content/www/us/en/embedded/products/apollo-lake/overview.html)
(formerly Apollo Lake).

Project Info: [Intel Developer Zone - Intel Atom® Processor E3900 Series
page](https://software.intel.com/en-us/articles/uefi-firmware-project-for-intel-atom-processor-e3900-series-processor-platforms)

Developers can download pre-built UEFI firmware images, utilities, binary object modules, and project release notes. The
open source firmware project is available from the TianoCore GitHub:

  [https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoard3-UDK2017](https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoard3-UDK2017)
  [https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoard3](https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoard3)

## MinnowBoard 3 Background

MinnowBoard is an open source hardware enabler, encouraging platform experimentation and derivative designs. The project
supports [Open Source Hardware Association](https://www.oshwa.org/) principles by making designs publicly available for
the community so “anyone can study, modify, distribute, make, and sell the design or hardware based on that design.”

MinnowBoard 3 was a follow-on to the [MinnowBoard Max](minnow_board_max.md) & MinnowBoard Turbot platforms. This design will
be migrated to [MinnowBoard 3 Module](intel_atom_processor_e3900.md) (release date TBD).

## Leaf Hill CRB

Leaf Hill refers to an Intel Customer Reference Board (CRB) using the [Intel Atom® Processor E3900
Series](https://www.intel.com/content/www/us/en/embedded/products/apollo-lake/overview.html) (formerly Apollo Lake).

Release 0.69 adds a separate release notes file with instructions for downloading the Leaf Hill CRB source tree and
compiling an IFWI image (which slightly differs from MinnowBoard 3).
