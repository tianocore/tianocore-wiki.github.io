# 2018 EDK II Capsule Hack-a-thon

## Overview

Part of [OSFC 2018](osfc_2018.md) in Erlangen, Germany
[https://www.osfc.io/](https://www.osfc.io/)
September 14-15 - times TBD

Participants are encouraged to exercise platform code related to
[`SignedCapsulePkg`](https://github.com/tianocore/edk2/tree/master/SignedCapsulePkg) and
[`FmpDevicePkg`](https://github.com/tianocore/edk2/tree/master/FmpDevicePkg) based on the
[`edk2-stable201808`](https://github.com/tianocore/edk2/releases/tag/edk2-stable201808) stable tag release.

This is the first time Intel has staged a public TianoCore hack-a-thon event. Thanks to the OSFC organizers for
providing the venue.

## Target Platforms

Test code provided is designed for the MinnowBoard Max & MinnowBoard Turbot platforms. These are open hardware platforms
based on 64-bit Intel® Atom™ E3800 Series processor family. [https://minnowboard.org](https://minnowboard.org)

Intel will have 15-20 platforms on site for testing, along with support hardware (SPI flash programmers, serial-to-USB
adapters, keyboards, etc.). Participants are expected to bring their own laptop systems for code compilation and
debugging.

## Target Codebase

Testing will be based on a project ported to the `edk2-stable201808` tag. This code will be available in github in
advance of the event (stay tuned). Since this is an updated version of `SignedCapsulePkg` and `FmpCapsulePkg`, the
official MinnowBoard firmware release does not have the latest implementation.

## Build Instructions

For this Hack-a-thon, there will be some additional build steps that are required. We have divided this up into a set of
instructions for [building on Windows](hackathon_build_windows.md) as well as [building on Linux](hackathon_build_linux.md).

## Participant Registration

Please review the [hack-a-thon participant
agreement](https://github.com/tianocore/tianocore.github.io/blob/master/files/TianoCoreHackathonRegistrationOSFC.pdf)
prior to OSFC 2018. This is a simple registration document that describes participant requirements and disclosure
guidelines. Note that vulnerability submissions may be eligible for the [Intel Bug Bounty
Program](https://www.intel.com/content/www/us/en/security-center/bug-bounty-program.html).

**Note: this document was updated on 2018-09-11 to remove the 'Confidentiality and Disclosure' clause due to concerns
from the community.**
**Note: this document was updated on 2018-09-11 to remove the 'Confidentiality and Disclosure' clause due to concerns
