# Coreboot Uefi Payload

## Outdated

Please see the updated page [UefiPayloadpkg](../core-packages/uefi_payload_pkg.md) that replaces the
CorebootModulePkg and CorebootPayloadPkg

### Coreboot

coreboot is an Open Source project aimed at replacing the proprietary
BIOS (firmware) found in most computers. coreboot performs a little bit
of hardware initialization and then executes additional boot logic,
called a payload. With the separation of hardware initialization and
later boot logic, coreboot can scale from specialized applications that
run directly from firmware, run operating systems in flash, load custom
bootloaders, or implement firmware standards, like PC BIOS services or
UEFI. This allows for systems to only include the features necessary in
the target application, reducing the amount of code and flash space
required. [https://www.coreboot.org/](https://www.coreboot.org/)

### Coreboot Payload Package

**CorebootModulePkg** - Source code package of Coreboot Support Modules
that will be used to parse the coreboot tables in memory , report
memory/io resources and install acpi and smbios tables from coreboot
into EFI system tables.

**CorebootPayloadPkg** - Source code package of Coreboot Payload
Modules, Provides definitions of payload image's layout and lists the
modules required in DSC file.

**Build Instructions**:
[https://github.com/tianocore/edk2/raw/master/CorebootPayloadPkg/BuildAndIntegrationInstructions.txt](https://github.com/tianocore/edk2/raw/master/CorebootPayloadPkg/BuildAndIntegrationInstructions.txt)

### Intel® Firmware Support Package

Intel holds the key programming information that is crucial for
initializing Intel silicon. The Intel® Firmware Support Package (FSP) is
a binary distribution of the silicon initialization code that is needed
to initialize the Intel silicon. The FSP will perform the necessary
initialization steps as documented in the BWG / BIOS Specification
including initialization of the CPU, memory controller, chipset and
certain bus interfaces, if necessary. The FSP provides chipset and
processor initialization in a format that can easily be incorporated
into many boot loaders including coreboot. [https://www.intel.com/fsp](https://www.intel.com/fsp)

### Payload

Coreboot in itself is "only" minimal code for initializing hardware. It
does not have the media drivers nor does it provide services required by
an OS. After the initialization, it jumps to a payload which can provide
services required to load and boot an OS.
[https://www.coreboot.org/Payloads](https://www.coreboot.org/Payloads)

![](https://raw.githubusercontent.com/tianocore/tianocore.github.io/master/images/Coreboot001.jpg)

### UEFI Payload

UEFI payload provides UEFI services on top of coreboot that allows UEFI
OS boot.

![](https://raw.githubusercontent.com/tianocore/tianocore.github.io/master/images/Coreboot002.jpg)
