# UEFI SCT

[UEFI](../../reference/specs-standards/uefi.md) \|
[PI](../../reference/specs-standards/pi.md) \|
[Testing
EDK II](../../reference/external-resources/edk_ii.md)

## UEFI Self-Certification Tests (SCT)

## UEFI SCT

The [UEFI](../../reference/specs-standards/uefi.md)
Self-Certification Test (UEFI SCT) is a toolset for platform developers
to validate firmware implementation compliance to the UEFI
Specification. The toolset features a Test Harness for executing
built-in UEFI Compliance Tests, as well as for integrating user-defined
tests that were developed using the UEFI SCT open source code.

The latest version of the UEFI SCT can be found at
[https://uefi.org/testtools](https://uefi.org/testtools)

## FWTS

The UEFI Board of Directors recommends the open source Firmware Test
Suite (FWTS) for [ACPI](../../reference/specs-standards/pi.md)
validation.

[https://wiki.ubuntu.com/FirmwareTestSuite](https://wiki.ubuntu.com/FirmwareTestSuite)

[https://wiki.ubuntu.com/FirmwareTestSuite/FirmwareTestSuiteLive](https://wiki.ubuntu.com/FirmwareTestSuite/FirmwareTestSuiteLive)

## UEFI PI SCT

The [PI](../../reference/specs-standards/pi.md) SCT is used to
perform self-certification testing on the PEI code.

The latest Version of the PI SCT can be downloaded from: [PI1.2 SCT.2
Release.zip](https://sourceforge.net/projects/pi-sct/files/PI1.2_Sct_.2_Release_Mar_05_2013/PI1.2-SCT.2Release-Mar052013.zip/download)

PI 1.2 introduces [SMM](../../reference/faqs-glossaries/acronyms_and_glossary.md) not
supported by EDK. PI SCT test cases for PI 1.2 must be built using
[EDK II](../../reference/external-resources/edk_ii.md). SCT
packages for PI 1.2+ must use EDK II coding style, as EDK will not be
supported from this point forward.

## Reference Documents

[UEFI SCT Case Writers Guide 91.
pdf](https://sourceforge.net/projects/edk2/files/General%20Documentation/UEFI%20SCT%20Case%20Writers%20Guide_0_91.pdf/download)
