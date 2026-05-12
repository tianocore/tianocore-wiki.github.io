# Platform Initialization (PI)

UEFI Platform Initialization (PI) describes the boot execution phases to encompass [UEFI](uefi.md) supported protocols
and services. PI evolved from the original Intel® Platform Innovation Firmware for EFI Specification. While UEFI
primarily focuses on the interface between firmware and the operating system, PI focuses on interfaces between low-level
firmware components.

The [PLATFORM INITIALIZATION WORK GROUP](https://uefi.org/workinggroups) (PIWG) of the UEFI Forum manages specification
development.

> "The PEI and DXE specifications authored by Intel and other related specifications as mutually agreed to by a
majority of the work group. Implementations of these specifications are intended to serve as firmware for initializing
a computer system and are intended to be implementations below the interface layer presented by the UEFI Specification.
Implementations of PEI and DXE would not be required for UEFI compliance." - [https://uefi.org/workinggroups](https://uefi.org/workinggroups)

More information: [UEFI & PI FAQ](../faqs-glossaries/uefi_pi_faq.md) | [PI Boot Flow](../../development/design-architecture/pi_boot_flow.md)
| [PI Boot Phases](https://raw.githubusercontent.com/tianocore/tianocore.github.io/master/images/PI_Boot_Phases.JPG)

![UEFI vs PI](https://raw.githubusercontent.com/tianocore/tianocore.github.io/master/images/UEFI_vs__PI_Spec.jpg)
