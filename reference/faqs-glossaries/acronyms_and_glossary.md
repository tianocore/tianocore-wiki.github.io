# Acronyms And Glossary

[Member-FAQ](../../community/support-onboarding/member_faq.md)

The EFI and Framework Open Source Community lives within a fairly
complex world of acronyms, terms, etc. Hopefully this page helps clarify
some of those and provides a good reference.

**ACPI**
Advanced Configuration and Power Interface. See <www.acpi.info>

**AL**
Afterlife phase. Also known as the "power down phase."

**AML**
ACPI Machine Language

**API**
Application Program Interface. Programmatic interfaces for the firmware
(not Win32-type OS-level APIs).

**a priori file**
A file with a known GUID that contains the list of DXE drivers that are
loaded and executed in the listed order before any other DXE drivers are
discovered.

**Artifact**
Something tracked in Project Tracker

**ASL**
ACPI Source Language

**Attribute**
A field of something tracked in CEE Project Tracker

**BA**
Boot Authorization

**BBS**
BIOS Boot Specification

**BDS**
Boot Device Selection phase

**BFV**
Boot Firmware Volume. Code (i.e., PEI and PEIM code) that appears in the
memory address space of the system without prior firmware intervention.
See also FV.

**BIS**
Boot Integrity Services

**BIST**
Built-in self test

**BLT**
Block Transfer (pronounced "blit" as in "slit" or "flit"). A series of
functions that form the basis of manipulation graphical data. The
operation used to draw a rectangle of pixels on the screen.

**BNF**
Backus-Naur Form. A metasyntactic notation used to specify the syntax of
programming languages, command sets, and the like

**BootDevice**
The device handle that corresponds to the device from which the
currently executing image was loaded

**BootManager**
The part of the firmware implementation that is responsible for
implementing system boot policy. Although a particular boot manager
implementation is not specified in this document, such code is generally
expected to be able to enumerate and handle transfers of control to the
available OS loaders as well as EFI applications and drivers on a given
system. The boot manager would typically be responsible for interacting
with the system user, where applicable, to determine what to load during
system startup. In cases where user interaction is not indicated, the
boot manager would determine what to load and, if multiple items are to
be loaded, what the sequencing of such loads would be.

**Boot Services**
The collection of interfaces and protocols that are present in the boot
environment. The services minimally provide an OS loader with access to
platform capabilities required to complete OS boot. Services are also
available to drivers and applications that need access to platform
capability. Boot services are terminated once the OS takes control of
the platform.

**BSD**
Berkeley Software Distribution

**BSP**
Boot Strap Processor - typically the processor that will execute SEC and
PEI

**COFF**
Common Object File Format. An (originally) Unix \*-based file format
that is now recognized under several OSs. The format uses one or more
header fields followed by the section data for the file

**Compatibility16**
A traditional legacy BIOS with the POST and BIOS Setup code removed.
Compatibility16 BIOS code executes in real mode

**CompatibilityBIOS**
The combination of both EfiCompatibility and Compatibility16

**CompatibilitySmm**
Any IBV-provided SMM code to perform traditional functions that are not
provided by EFI

**CRC**
Cyclic Redundancy Check. A fixed-size error checking code appended to
the end of a block of data (file) that is based on the content of the
file

**CRTM**
Core Root-of-Trust Module

**CSM**
Compatibility Support Module. The combination of EfiCompatibility,
CompatibilitySmm , and Compatibility16. Portion of the Framework that
allows compatibility with non-EFI compliant operating systems to run on
Framework firmware

**CVDR**
Configuration Values Driven through Reset

**Depex**
Dependency expression. Code associated with each Framework driver that
describes the dependencies that must be satisfied in order for that
driver to run. Controls order of execution in a Framework dispatch of
PEIM and DXE drivers

**DispatchEntry Point**
The entry point that the dispatcher invokes

**Driver**
Modular chunk of firmware code that supports chipset or platform
features. bReusable in multiple system contexts

**DXE**
Driver Execution Environment phase

**DXE Foundation**
A set of intrinsic services and an execution mechanism for sequenced
control of driver modules

**DXE Services**
Services, such as security services and driver services, that are usable
by DXE drivers

**EfiCompatibility**
EFI code that corresponds to EFI compatibility drivers, code that
generates data for compatibility interfaces, or code that invokes
compatibility services.

**EDK**
EFI Developer Kit

**EPL**
Eclipse Public License

**[Extensible Firmware
Interface](Wikipedia:Extensible_Firmware_Interface) (EFI)**
EFI is a specification that defines the interface between an operating
system, option ROMs, and platform firmware. EFI was originally invented
by Intel as the [Intel(R) Itainum](https://en.wikipedia.org/wiki/Itanium)
BIOS replacement. EFI is now owned by a industry non-profit
collaborative trade organization called [The UEFI
Forum](https://www.uefi.org).

**FAT**
File Allocation Table

**FAT32**
FAT32 File System Driver

**FD**
Firmware Device. A persistent physical repository that contains firmware
code and/or data and that may provide NVS. For the purposes of this
architecture specification, the topology of bFDs should be abstracted
via bFVs.

**FFS**
Firmware File System. A binary storage format that is well suited to
firmware volumes. The abstracted model of the FFS is a flat file system

**Firmware Device**
See FD.

**Firmware Volume**
See FV.

**FIT**
Firmware Interface Tableb.( Itanium systems only)

**Font**
A translation between Unicode weights and glyphs. This "M" and this "M"
and this "M" represent the same weight but in different fonts

**FoundationCode**
The core interoperability interfaces between modules and in the
Framework

**FPSWA**
Floating Point Software Assist. (Itanium systems only)

**Framework**
short for Intel® Platform Innovation Framework for EFI

**FS**
Firmware Store. The abstracted model of the FS is a flat "file system"
where individual files are SUMs

**FV**
There are one or more FVs in the FS. The FV containing the "reset
vector" is known as the Boot Firmware Volume (BFV). A FV is a simple
Flash File System that starts with a header and contains files that are
named by a GUID. The file system is flat and does not support
directories. Each file is made up of a series of sections that support
encapsulation.

**GCD**
Global coherency domain. The address resources of a system as seen by a
processor. It consists of both system memory and I/O space

**glyph**
The graphical representation of a single Unicode weight

**[Globally Unique IDentifier](Wikipedia:_Globally_Unique_Identifier) (GUID)**
Globally Unique Identifier. A 128-bit value used to differentiate and
name services and structures.

**HII**
Human Interface Infrastructure. Repository of configuration and
translation information for localization. Typically used with boot
manager and shell to provide a localized user interface.

**HOB**
Hand-Off Block. A structure used to pass information from one boot phase
to another (i.e., from the PEI phase to the DXE phase)

**IBV**
Independent BIOS Vendor

**IFR**
Internal Forms Representation. A binary encoding of forms-based display
content and configuration information

**IHV**
Independent Hardware Vendor

**IME**
Input Method Editor

**Intrinsic Services**
Services, such as security services and driver services, that remain
available after the phase during which they are instantiated

**IPL**
Initial Program Load. An architectural PEIM to PEIM interface that
starts the DXE phase

**IPMI**
Intelligent Platform Management Interface

**ISO 3166**
An association between a country or region and a two or three character
ASCII string

**ISO 639-2**
An association between a language or dialect and a three character ASCII
string

**Localization**
Concepts by which an interface is made useful to users speaking
different languages and from various cultures by adapting the interfaces
to the user. "STOP" in English would be "ALTO" in Spanish and "СТОП" in
Russian. Alphabetic on keyboards are local to the language and may be
local to the country the keyboard is localized for. For example, a
French keyboard in France is different from a French keyboard in Canada.

**MCA**
Machine Check Architecture

**MDE**
Module Development Environment

**NMI**
Non-maskable Iinterrupt

**NRAM**
Nonvolatile Random Access Memory

**NVS**
Nonvolatile storage. Flash, EPROM, ROM, or other persistent store that
will not go away once system power is removed

**ODM**
Original Device Manufacturer

**OEM**
Original Equipment Manufacturer

**OpROM**
Option ROM

**PAL**
Processor Abstraction Layer. A binary distributed by Intel that is used
by the 64 bit Itanium processor family

**PCI**
Peripheral Component Interconnect. See <www.pcisig.com> for more
information.

**PCR**
Platform Configuration Register

**PE/COFF**
PE32, PE32+, or Common Object File Format. A defined standard file
format for binary images

**PEI**
Pre-EFI Initialization phase. Set of drivers usually designed to
initialize memory and the cpu so that DXE phase can run. sually the
first bset of code run starting from reset.

**PEI Foundation**
A set of intrinsic services and an execution mechanism for sequenced
control of PEIMs

**Pre EFI Initialization Module (PEIM)**
Pre-EFI Initialization Module. Modular chunk of firmware code running in
PEI that supports chipset or platform features. Reusable in multiple
system contexts.

**PEI Services**
Common services that are usable by PEIMs

**PEIM to PEIM Interface (PPI)**
A C structure named by a GUID that is published by one PEIM and consumed
by another. The C structure can contain data and member functions. It
differs from a Protocol in that it may have to function prior to memory
being available and parts of the PPI could be in read only memory.

**PHIT**
Phase Handoff Information Table. A HOB that describes the physical
memory used by the PEI phase and the boot mode discovered during the PEI
phase.

**PIC**
Position-independent code. Code that can be executed at any address
without relocation

**POST**
Power On Self Test

**Protocol**
A C structure named by a GUID that is published by one EFI or DXE driver
and consumed by another. The C structure can contain data and member
functions.

**Reverse Thunk**
The code to transition from 16-bit real mode to native execution mode

**RSD_PTR**
ACPI definition: Root System Description Pointer

**RT or Runtime phase**
For EFI and the Framework this is after exit boot services has executed
and the OS is in control of the system.

**Runtime Services**
Interfaces that provide access to underlying platform-specific hardware
that may be useful during OS runtime, such as time and date services.
These services become active during the boot process but also persist
after the OS loader terminates boot services.

**SAL**
System Abstraction Layer. (Itanium systems only)

**SALE_ENTRY**
System Abstraction Layer entry point. (Itanium systems only)

**Sandbox**
The common properties of a driver or preboot environment that allow
applications to run. These properties include a defined load image
format and services that can run in the sandbox.

**SEC**
SECurity Phase. Initial starting point for boot process, first code
executed after hardware reset. Responsible for 1) Establishing root
trust in the software space; 2) Initializing architecture specific
configuration to establish memory space for the C code stack.

**SMI**
System Management Interrupt

**SMM**
System Management Mode

**SOR**
Schedule on Request

**SSE**
Streaming SIMD Extensions

**SUM**
Separately Updateable Module. A portion of the BFV that is treated as a
separate module that can be updated without affecting the other SUMs in
the BFV.

**Tiano**
Codename for the Intel Project to develop the Framework

**TCB**
Trusted Computing Base

**TCG**
Trusted Computing Group

**TE Image**
Terse Executable image. An executable image format that is specific to
the Framework. This format is used only in PEI and is used for storing
executable images in a smaller amount of space than would be required by
a full PE32+ image. Is a smaller more compact version of bPE32.

**Thunk**
The code to transition from native execution mode to 16-bit real mode

**UNDI**
Universal Network Driver Interface. Silicon specific driver in the
preboot LAN stack that interfaces to SNP and PXEBC

**Unicode**
A standard defining an association between numeric values known as
"weights" and characters from the majority of the worlds currently used
languages. See the Unicode specification for more information.

**USB**
Universal Serial Bus. See [https://www.usb.org](https://www.usb.org) for more information

**VFR**
Visual Forms Representation. A high-level language representation of IFR

**VM**
Virtual Machine

**VTF**
Volume Top File. A file in a firmware volume that must be located such
that the last byte of the file is also the last byte of the firmware
volume

**VT-UTF8**
A serial protocol definition that extends VT-100 to support Unicode

**Watchdog Timer**
An alarm timer that may be set to go off. This can be used to regain
control in cases where a code path in the boot services environment
fails to or is unable to return control by the expected path.

**XIP**
Execute In Place. PEI code that is executed from its storage location in
a firmware volume

------------------------------------------------------------------------

[EDK II FAQ](edk_ii_faq.md)
Frequently asked questions about EDK II

[UEFI/PI
FAQ](uefi_pi_faq.md) Frequently asked questions about UEFI/PI
