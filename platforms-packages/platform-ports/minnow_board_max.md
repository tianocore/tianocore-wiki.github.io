# MinnowBoardMax

[EDK II Platforms](edk_ii_platforms.md) | [Intel® Processor Platforms](edk_ii_platforms.md#intel-processor-platforms)

***

## MinnowBoard Max/Turbot

![](https://minnowboard.org/wp-content/uploads/2017/10/MBTurbot-dual-core-Top-0001-171002-1.png)

MinnowBoard Max & Turbot are low cost, commercially available, reference platforms for hardware, software and firmware
developers who wish to work within an open environment. Design specifications and materials have been provided to the
open community, encouraging platform experimentation and derivative designs.

The project is based on Intel® Atom™ processors. Technical details, schematics, and information on expansion boards
(Lures) can be found at [https://minnowboard.org](https://minnowboard.org)

A list of currently supported boards and prices can be found at: [https://minnowboard.org](https://minnowboard.org)
under the "[Boards](https://minnowboard.org/compare-boards)" tab

### UEFI Firmware - Release

Documentation, binary images, and source build instructions are at
[https://software.intel.com/en-us/articles/minnowboard-maxturbot-uefi-firmware](https://software.intel.com/en-us/articles/minnowboard-maxturbot-uefi-firmware)

* Binary Firmware Images ("UEFI BIOS"): 32-bit and 64-bit UEFI builds, with Debug and Release images. Includes flash
  update utilities and release notes.
* Binary Object Modules for which source code is not available due to the Intellectual property for both Intel silicon
  (microprocessor and chipset) and third party components.
* How to build Instructions for integrating the EDK II source tree (BSD license) with the Binary Object Modules to build
  firmware. See the latest **Release Notes .txt** file at [Intel Developer Zone -Minnowboard-max Firmware
  Releases](https://software.intel.com/en-us/articles/minnowboard-maxturbot-uefi-firmware)
  * How to build with FSP
  * How to configure Memory Parameters
  * How to enable fTPM feature
* The open source firmware project is available from the TianoCore GitHub:
    [https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoardMax-UDK2017](https://github.com/tianocore/edk2-platforms/tree/devel-MinnowBoardMax-UDK2017)

### UEFI Firmware - Development

The EDK II development firmware for the MinnowBoard Max / Turbot is now available. Please see the [Valleyview2
Readme](https://github.com/tianocore/edk2-platforms/blob/master/Platform/Intel/Vlv2TbltDevicePkg/Readme.md) for
instructions to download and build a firmware image. This development firmware is ECP free and can cowork with the
latest updates from EDK II.
