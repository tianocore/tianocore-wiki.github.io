# Intelatomprocessore3900

[EDK II Platforms](edk_ii_platforms.md) | [Intel® Processor Platforms](edk_ii_platforms.md#intel-processor-platforms)

***

## UEFI Firmware Project for Intel Atom® Processor E3900 Series Processor Platforms (Leaf Hill, Up Squared & MinnowBoard
3 Module)

This project is open source UEFI firmware, based on the TianoCore EDK II codebase, for the following platforms based on
the Intel Atom® Processor E3900 Series processor (formerly Apollo Lake).

* Leaf Hill Customer Reference Board (CRB)
* Up Squared by UP-BOARD (commercially available maker board, see "Supported Platforms" below)
* MinnowBoard 3 Module (Pre-production Board, ship date TBD)

Developers can download pre-built UEFI firmware images, utilities, binary object modules, and project release notes. The
open source firmware project is available from the TianoCore GitHub:

  [https://github.com/tianocore/edk2-platforms/tree/devel-IntelAtomProcessorE3900](https://github.com/tianocore/edk2-platforms/tree/devel-IntelAtomProcessorE3900)
  [https://software.intel.com/en-us/articles/uefi-firmware-project-for-intel-atom-processor-e3900-series-processor-platforms](https://software.intel.com/en-us/articles/uefi-firmware-project-for-intel-atom-processor-e3900-series-processor-platforms)

## Reporting Firmware Issues

Please report any firmware issues in [GitHub Issues](https://github.com/tianocore/edk2-platforms/issues) using the
following field values:

* Product: EDK2 Platforms
* Component: Minnowboard 3

See [Reporting Issues](../../community/support-onboarding/reporting_issues.md) for more information.

## Supported Platforms

### Leaf Hill CRB

Leaf Hill refers to an Intel Customer Reference Board (CRB) using the [Intel Atom® Processor E3900
Series](https://www.intel.com/content/www/us/en/embedded/products/apollo-lake/overview.html) (formerly Apollo Lake).

### Up Squared by UP-BOARD

Support for the UP Squared board was introduced in Release 0.71. Pre-built images and build instructions are available
on [Intel Developer Zone -
Firmware](https://software.intel.com/en-us/articles/uefi-firmware-project-for-intel-atom-processor-e3900-series-processor-platforms).
Please refer to the [release
notes](https://software.intel.com/en-us/articles/uefi-firmware-project-for-intel-atom-processor-e3900-series-processor-platforms)
for information on platform features and known issues.

_**NOTE:** The firmware provided by this project for the UP Squared maker board is not based on the official
manufacturer's firmware. This project is for experimentation and is not supported by the manufacturer (Aaeon). Flashing
this firmware on the UP Squared will void the manufacturer's warranty. Thanks to Aaeon for their cooperation and
providing platform porting information._

More Info:

* [BIOS chip flashing on UP Squared](https://wiki.up-community.org/BIOS_chip_flashing_on_UP_Squared) - Information for
  using a SPI programmer to apply a new firmware image (factory image or custom image)
* Product Info: [https://www.up-board.org/upsquared/](https://www.up-board.org/upsquared/) &
  [https://www.up-china.net/](https://www.up-china.net/)

### MinnowBoard 3 Module

MinnowBoard 3 Module is the follow-on to the [MinnowBoard Max](minnow_board_max.md) & MinnowBoard Turbot platforms.
MinnowBoard platforms offer low cost & commercially available open hardware based on Intel Architecture for hardware,
software and firmware developers. Hardware availability is TBD.

MinnowBoard is an open source hardware enabler, encouraging platform experimentation and derivative designs. The project
supports [Open Source Hardware Association](https://www.oshwa.org/) principles by making designs publicly available for
the community so “anyone can study, modify, distribute, make, and sell the design or hardware based on that design.”

MinnowBoard 3 Module is based on the Intel Atom® processor E3900 Series platform, utilizing the Intel® Firmware Support
Package (Intel® FSP) and open source UEFI from the TianoCore EDK II project.

## Note: Previous Codebase for MinnowBoard 3

Intel Atom® Processor E3900 Series Processor Platforms were originally supported by the [MinnowBoard
3](minnowboard_3.md) codebase. At release 0.70, this codebase was updated to use
[UDK2018](../../releases-history/archives/udk2018.md) and moved to a new branch with broader platform support.
