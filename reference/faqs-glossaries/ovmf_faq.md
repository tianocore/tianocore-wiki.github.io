# OVMF FAQ

- [What is Open Virtual Machine Firmware
    (OVMF)?](#what-is-open-virtual-machine-firmware-ovmf)
- [What source code license does OVMF
    use?](#what-source-code-license-does-ovmf-use)
- [What does OVMF provide?](#what-does-ovmf-provide)
- [Are OVMF releases fully UEFI
    compliant?](#are-ovmf-releases-fully-uefi-compliant)
- [Does OVMF support legacy booting, legacy option ROMs,
    CSM?](#does-ovmf-support-legacy-booting-legacy-option-roms-csm)
- [What quality level is the OVMF code? (Alpha,
    Beta, Production?)](#what-quality-level-is-the-ovmf-code-alpha-beta-production)
- [What virtual machines are supported by the OVMF firmware
    builds?](#what-virtual-machines-are-supported-by-the-ovmf-firmware-builds)
- [Are only open source virtual machines
    supported?](#are-only-open-source-virtual-machines-supported)
- [How can I use OVMF with a VM?](#how-can-i-use-ovmf-with-a-vm)
- [Where is the source code to
    OVMF?](#where-is-the-source-code-to-ovmf)
- [If I work on a Virtual Machine, how can I make use of
    OVMF?](#if-i-work-on-a-virtual-machine-how-can-i-make-use-of-ovmf)
- [Where can I find the current status and roadmap of
    OVMF?](#where-can-i-find-the-current-status-and-roadmap-of-ovmf)
- [What build environments are supported by
    OVMF?](#what-build-environments-are-supported-by-ovmf)
- [Where can I ask additional questions and discuss
    OVMF?](#where-can-i-ask-additional-questions-and-discuss-ovmf)
- [How can I contribute to OVMF?](#how-can-i-contribute-to-ovmf)
- [How do I build OVMF?](#how-do-i-build-ovmf)
- [How do I enable source level debugging with
    OVMF?](#how-do-i-enable-source-level-debugging-with-ovmf)

### What is Open Virtual Machine Firmware (OVMF)?

OVMF is a project to enable support for UEFI within Virtual Machines. It
is built upon the EDK II code base.

### What source code license does OVMF use?

OVMF follows the EDK II project in using the open source [BSD+Patent](../legal-licenses/bsd_plus_patent_license.md) license.
Some OVMF content is convered by additional licenses detailed in
[OvmfPkg/License.txt](https://github.com/tianocore/edk2/blob/master/OvmfPkg/License.txt).

### What does OVMF provide?

- Libraries and drivers related to virtual machines
- An entire firmware implementation with supports UEFI on open source
    virtual machines.

### Are OVMF releases fully UEFI compliant?

While the goal is to be as fully UEFI compliant as possible, you should
not assume that an OVMF release is fully UEFI compliant unless the
particular release states full compliance.

For virtual machines, there are some challenging areas in achieving full
UEFI compliance. For example, UEFI 'non-volatile' variables may be
difficult to fully support in some virtual machine environments if a
flash memory device is not emulated.

### Does OVMF support legacy booting, legacy option ROMs, CSM?

No. While OVMF may have UEFI firmware drivers for some legacy hardware,
there is no planned legacy compatibility for OVMF. One reason for this
is that there is currently no open source CSM module which could be used
within OVMF. Another reason is that we would like to use OVMF to help
drive out some legacy assumptions which might be made by software layers
above the system firmware.

### What quality level is the OVMF code? (Alpha, Beta, Production?)

The quality level of the OvmfPkg within the edk2 source code repository
may change over time. Therefore, the best way to determine the current
quality level of the source repository is to look at the README document
under the OvmfPkg source directory or ask the EDK II dev email list.
Binary releases of OVMF may indicate a code quality level, and should
also indicate the versions of the source code repositories used to
produce that binary release.

### What virtual machines are supported by the OVMF firmware builds?

Hopefully this will improve over time, but initially the QEMU virtual
machine will be supported while emulating a IA32 (x86) or X64 (x86-64)
based system. The README text file included under the OvmfPkg source
tree should contain the most up to date list of supported virtual
machine environments. The README text file included with binary releases
should also document the supported virtual machine environments which
are supported for that release.

### Are only open source virtual machines supported?

We have chosen the BSD+Patent license to enable easy incorporation of any of
the piece of our code within nearly any type of product. But, it is
likely that the firmware images produced by OvmfPkg under edk2 will only
support open source virtual machines.

### How can I use OVMF with a VM?

Pre-built binaries of OVMF are available under the 'OVMF' folder in the
'Documents & files' area of the EDK II project. The binaries of OVMF are
intended to replace the normal firmware/bios that a VM would use when
booting the VM. The README file included in the downloaded archive file
will explain how to run the OVMF firmware image with the supported VM.

More detailed instructions for running OVMF can be found on the [running
OVMF](../../platforms-packages/platform-ports/galileo.md) wiki page.

### Where is the source code to OVMF?

The source code to OVMF is under the OvmfPkg directory within the EDK II
source repository. Changes to other EDK II packages (directories) will
be driven by the OVMF project as required.

### If I work on a Virtual Machine, how can I make use of OVMF?

At the minimum, you can utilize OVMF as a sample platform for how a VM
firmware can be built with EDK II. If you find that the OVMF platform
and/or EDK II code base does not provide adequate support for building
your VM firmware, please open a discussion on the EDK II dev email list.

### Where can I find the current status and roadmap of OVMF?

Please look in the README file under the OvmfPkg source directory.

### What build environments are supported by OVMF?

OVMF's goal is to support all the build environments which the main EDK
II project supports. This includes support for building under the Linux,
Mac OS X and Windows operating systems. Toolchain support includes GCC,
Visual Studio (2003 or 2005), WINDDK, ICC (Intel Compiler) and GCC under
CYGWIN.

If you are new to EDK II development, [this
page](../../platforms-packages/platform-ports/galileo.md) may help you get a build
environment up and running. OVMF will also require an ASL compiler to be
installed on the system. The Intel ASL compiler is compatible with many
operating systems, and is available from [https://www.acpica.org](https://www.acpica.org).

### Where can I ask additional questions and discuss OVMF?

Questions and discussion related to OVMF should be directed to the EDK
II dev email list.

### How can I contribute to OVMF?

Please refer to the [Getting Started](../../platforms-packages/platform-ports/galileo.md) page.

### How do I build OVMF?

Refer to the README document under the OvmfPkg directory within the EDK
II source repository or on the [building
OVMF](../../platforms-packages/platform-ports/galileo.md) wiki page.

### How do I enable source level debugging with OVMF?

Please refer to:

  1. [EDK II Source Level Debug](../../development/tutorials-howto/edk_ii_source_level_debug.md)
  2. [How to debug OVMF with QEMU using GDB](../../development/tutorials-howto/how_to_debug_ovmf_with_qemu_using_gdb.md)
3. [How to debug OVMF with QEMU using
WinDbg](../../development/tutorials-howto/how_to_debug_ovmf_with_qemu_using_windbg.md)
