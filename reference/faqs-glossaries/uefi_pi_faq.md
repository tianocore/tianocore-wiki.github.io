# UEFI PI FAQ

More information: [UEFI-and-PI-Wiki](../external-resources/uefi_and_pi_wiki.md) \|
[PI Boot
Flow](../../development/design-architecture/pi_boot_flow.md) \| [PI](../specs-standards/pi.md)

**Frequently asked Questions about UEFI/PI**

[Build](../../build-tooling/build-workflows/build.md) questions

[Depex FAQ](depex_faq.md)
questions

[GPT FAQ](gpt_faq.md) questions

[GUID FAQ](guid_faq.md)
questions

[HII](../../platforms-packages/component-guides/hii.md) questions

[PEI](pei.md) questions

[Shell](../../platforms-packages/component-guides/shell.md) questions

[UEFI application](../../development/tutorials-howto/uefi_application.md) questions

[UEFI
Drivers](../specs-standards/uefi.md)

### It seems as though UEFI is revolutionary given the scope of changes from legacy BIOS

The Implementation may seem like it is revolutionary since it is now in
“C” instead of Assembly but once the structure of UEFI is understood it
is then easier to see how UEFI is doing some of the same work as Legacy
BIOS. In fact, the UEFI implementations are done with software design
structures in mind.

Two other notes on this:

- UEFI / PI accounts for legacy BIOS through the CSM
- UEFI was originally a layer built on top of legacy bios giving the
  interfaces for UEFI option ROMs and UEFI Drivers.

### Is there support for multiple processor architectures?

Currently there is support for the Intel X86 Family (IA32 and X64) as
well as the Intel® Itanium® Processor Family (IPF). Other companies are
supporting the ARM processor

### What is the UEFI adoption rate of UEFI Platforms on the market?

Currently over 50% maybe somewhere above 70% as of 2010

### Can Self Certification Tests (SCT) be extended?

Yes, the UEFI SCT User Guide has a section (5.3) on how to add test
cases to the SCT. The SCT releases can be downloaded from
[https://uefi.org](https://uefi.org)

### Who defines Protocols?

The UEFI and PI specifications define many useful protocols available to
be consumed by developers. If you are writing a driver there may be
other protocols that your diver will produce. In general protocols are
function calls with a well defined interface from either the UEFI or PI
specifications.

### What is the relationship between boot and Runtime services?

Boot services are available only until ExitBootServices() is invoked.
Whereas Runtime services are available during boot and after
ExitBootServices().

Runtime services are intended to provide additional support to the OS
after the pre-boot environment is no longer relevant. SMM code on Intel
Processors is considered Runtime code, by its very nature(it is not
feasible to remove SMM code from SMM space one established. Ergo SMM
code will remain resident after ExitBootServices().

### What about boot and runtime Services in SMM mode?

The SMM runtime services are similar to the DXE foundation but SMM
libraries would only contain SMM code.

SMM code on Intel Processors is considered Runtime code, by its very
nature(it is not feasible to remove SMM code from SMM space one
established. Ergo SMM code will remain resident after
ExitBootServices().

### Add-in cards need to support legacy image as well as EBC. Any conversion tools?

There are no tools to help convert a Legacy Option ROM sources or
binaries to a UEFI complaint driver. Once a UEFI complaint driver is
designed and implemented, it may be cross compiled to EBC. There are
some compatibility issues that the UEFI driver writer must be aware of
in order to implement a UEFI driver is C that is compatible with native
mode compilers as well as the EBC compiler. Chapter 19 of the EFI Driver
Writers Guide at [https://developer.intel.com/technology/efi/dg.htm](https://developer.intel.com/technology/efi/dg.htm)
describes the porting considerations when using the EBC compiler.

### Can you show the control flow from power-on to OS boot?

Yes, per the PI Spec the UEFI / PI firmware goes through the following
phases:

- Sec – Security phase – Set up Temp memory
- PEI – Pre-EFI – Initialize Memory
- DXE – Driver Execution – Dispatch list of UEFI / DXE drivers
- BDS – Boot Device Selection –Run Setup
- TSL- Transient System Load – Run EFI Shell
- Runtime boot to the OS – Execute the OS loader

### What is PI? Is it the same as legacy?

No, PI stands for Platform Initialization and the original Intel®
Platform Innovation Firmware for UEFI Specification it evolved into the
Platform Initialization (PI) Specification. PI describes the boot
execution phases to encompass UEFI supported protocols and services.

### There are UEFI defines services and there are PI Services. Do any of these services overlap and are there services?

The PI services are a super set of services so that the UEFI Services
are all part of PI services. In addition there are some PEI services
specific to PEI since PEI is not executing out of memory.

### What are events?

UEFI events are constructs that are used to provide communication
between UEFI modules.

Timer events are asynchronous. All other event types are synchronous.

UEFI Events are managed through UEFI Services. UEFI Events can be
created and destroyed and are either in the waiting state or the
signaled state. An UEFI image can do any of the following:

- Create an event.
- Destroy an event.
- Check to see if an event is in the signaled state.
- Wait for an event to be in the signaled state.
- Request that an event be moved from the waiting state to the signaled
  state.

### Priorities between events?

Every event will have a Task Priority Level (TPL) associated with it.
The TPL levels are

- TPL_APPLICATION
- TPL_CALLBACK
- TPL_NOTIFY
- TPL_HIGH_LEVEL

### How does HW signal if not Interrupts (INTS)?

Instead, UEFI supports polled drivers. The most common use of events by
an UEFI driver is the use of timer events that allows drivers to poll a
device periodically. So any device I/O is done through polling.

### So UEFI does not discover interrupt routing & program apics?

Discovery, yes but typically a UEFI Platform will only program these if
it is going to boot a legacy OS. This resource information is however,
discovered during PEI and DXE. It would later handshake to the OS
through the CSM. Much of this is done at the platform level through the
ACPI data structures.

### How does UEFI know which is the boot device, so it can enumerate it? What if there is no user intervention?

The Boot devices are determined by the UEFI boot manager. The UEFI boot
manager is a firmware policy engine that can be configured by modifying
architecturally defined global NVRAM variables. The first boot device,
is typically set by OEM, is in the UEFI NVRAM Variable BootOrder

### Can C++ be used?

No, C++ has a larger footprint, and the UEFI environment does not
support constructors and destructors. The “C” code can be used to do
similar C++ things by writing specific algorithms in C. There is No C++
support. However, you can use C++ code as long as there are no global
objects—do not use new .

### How does No Eviction Mode (NEM) work?

For Intel X86 Processors early in the SEC phase the MTRRs are set up to
define the stack just under 4G but in cache. The processor is then set
to “No Eviction Mode”. Then the Stack pointer (SP) is set to a location
in this memory.

This allow the Cache to be used for Data accesses, as if there were RAM
at those addresses. The cache is not “flushed” (no eviction). When RAM
is available, the cache can be evicted into the physical RAM space
assigned for cache, thus providing a ‘seamless’ context change of CAR to
RAM mode.

### On Intel Itanium® Processor Family (IPF) Servers with Firmware interface table FIT – a table at an architected
location pointing to pal & sal components is it similar?

FIT on IPF is architectural. It is described in the public SDM for IPF.
Describes the location of PAL/SAL. It is not related to NEM.

### When there are multiple Firmware Volumes (FVs), what additional code is required?

The PI PEI core foundation already has a mechanism for handling multiple
FVs. Multiple FVs is platform specific and would be described in a
platform’s FDF file.

A platform specific PEIM has to declare the location of additional FVs
for them to be known to the PEI Core. Once they are declared to the PEI
Core, the PEI Core can dispatch PEIMs from them. The only FV that is
known to the PEI Core when it is started is the Boot Firmware Volume
(BFV). The BFV contains SEC and the PEI Core and one or more PEIMs.

### Do some Servers have multiple FVs for redundancy?

Yes, this is maybe a platform policy and it would be platform policy of
the mechanism for using the FVs for redundancy. That mechanism is not
scoped within the PI spec.

### Are there any limits on memory size?

Just 64bit address space . . .

### What about TSEG?

TSEG would be platform specific and this information is passed in some
HOB, EFI_SMRAM_HOB_DESCRIPTOR_BLOCK.

### Are timers run off apics, . . . or implementation & cpu architecture dependent?

The method is really Implementation dependant, but in Intel
architectures it models an 8254 timer API

### Is it a global time for all events?

Yes.

### Referring to PI Vol. 2 Section 10.11 DXE Dispatcher State Machine what is SOR? How does it work?

SOR Stands for Schedule On Request. If the SOR opcode is present in a
DXE driver’s dependency expression, then the DXE driver is placed in the
“Unrequested” state. If the SOR opcode is not present in the DXE
driver’s dependency expression, then the DXE driver is placed in the
“Dependent” state

When SOR is present, the module will not be executed until another
driver specifically requests that driver to be run. This is done through
the DXE Service called Schedule(). See PI 1.2 DXE CIC Section 7.3

### Is NMI the same?

It depends on NMI is mapped in the platform. In the PC environment, the
NMI is part of the standard hardware interrupts of the processor. In
fact, the support hardware of a PC can actually disable the NMI source
through the chipset (so much for “non-Maskable Interrupt”). It was the
need for a truly non-Maskable firmware managed interrupt that brought
about the SMI system.

### Can DXE register additional SMI handlers later?

SMM Drivers are dispatched by the SMM Core during the DXE Phase. So
additional SMI handlers can be registered in the DXE Phase. Late in the
DXE Phase, when no more SMM drivers can be dispatched, SMRAM will be
locked down (as recommended practice). Once SMRAM is locked down, no
additional SMM Drivers may be dispatched, so not additional SMI handlers
can be registered. For example, an SMM Driver that registers an SMI
handler cannot be loaded from the EFI Shell or be added as a
DriverOption in the UEFI Boot Manager.

### Are boot options defined PI spec?

No, in the UEFI spec.

### What is an example of the flash organization from a real platform?

1. FV Recovery
2. Ftw Spare Space
3. Ftw Working Space
4. Event log
5. Microcode
6. Variable Region
7. FV Main

### What is the Hierarchy of flash device FD, Firmware Volumes FV, Firmware File System FFS, EFI files?

Flash device FD –\> FV -\> FFS -\> EFI Files. Multiple EFI Sections are
combined into a Firmware file (FFS) which consists of zero or more EFI
sections. Each FFS consists of a FFS header plus the data One or more
FFS files are combined into a Firmware Volume (FV.) One or more FV would
be listed as part of the Flash Device

### What specifies flash org?

The flash layout is defined in the .FDF file for the platform

### Is the BDS from PI?

The BDS phase is part of the PI spec. and it is responsible for
implementing the platform boot policy. However, the BDS phase System
firmware that is compliant with the PI specification must implement the
boot policy specified in the Boot Manager chapter (chapter 3) of the
UEFI 2.x specification

### What about Microsoft Windows Blue Screen of Death?

Currently Win 7 still requires CSM for blue screen and Win 7
installations. Microsoft has indicated that there will be a fix in Win7
sp2

### Where are FVs or firmware Volumes contained?

FV contained inside one or more flash devices.

### What is the difference between EFIDISKIOPROTOCOL and EFIBLOCKIOPROTOCOL?

The EFI_BLOCK_IO_PROTOCOL abstracts mass storage devices to allow code
running in the UEFI boot services environment to access them without
specific knowledge of the type of device or controller that manages the
device. Functions are defined to read and write data at a block level
from mass storage devices as well as to manage such devices in the EFI
boot services environment. To contrast, the EFI_DISK_IO_PROTOCOL is used
to abstract the block accesses of the Block I/O protocol to a more
general offset-length protocol. The firmware is responsible for adding a
EFI_DISK_IO_PROTOCOL to any Block I/O interface that appears in the
system that does not already have a Disk I/O protocol. File systems and
other disk access code utilize the Disk I/O protocol

Disk I/O provides byte level access to a disk.

### Why is the order of which drivers are installed into handle database not deterministic?

Within the boot execution phase the UEFI firmware will assign a handle
as each image is loaded. If something changes in the configurations then
that may cause a different handle number to be assigned to the same
driver. However, it is likely to be the same on each boot if nothing
changes from boot to boot.

### Can UEFI and OS be in different modes?

UEFI and OS need to be in the same mode—32 bit or 64 bit

### Any formal forum?

Yes, Join the Sourceforge and sign up for the developer for EDK II.
[https://sourceforge.net/projects/edk2/develop](https://sourceforge.net/projects/edk2/develop) and subscribe to the
mailing lists. Get involved with the UEFI Forum at [https://www.uefi.org](https://www.uefi.org)

### Given a FV / ROM image is there tools to find out the layout and they way they are packed?

There are new tools for EDK II. The PI Specification for firmware
Volumes (FV) and FFS is different than the Framework .9 Specification FV
and FFS. EDK II is using the PI Spec FV and FFS. The old tools can only
work on EDK I that use the Framework .9 Spec. VolInfo is a utility which
displays the contents of a firmware volume residing in a file for
informational purposes. Command line in Basetools/Bin/Win32

### Is there a mechanism to sign components and is there a tool to determine which components are signed?

There is a mechanism in EDK II that is utilizing the encapsulation
section in the PI spec. The Encapsulation Section for a FV can be used
for different things such as compression, signing and for encryption.
The support in the build tools for EDK II is PI 1.2 Spec. so if signing
is needed then the Build tools for EDK II will work Single FFS with a
signed section Example – in EDK II – Tiano compress and LZA compression
– There are GUIDs in the FDF file You can define a new GUID for a new
encapsulation type. Encode/ Decode operations. When the platform needs
to decode there will be a library to perform the decode.

### How is interrupt hooking mechanism done in EFI?

There are no hardware interrupts in EFI. All that exists is a timer.

Basically you use the CreateEvent() boot service to make a timer event,
and then you do a SetTime() boot service call to program the period and
type of the event. You can pass a Notify function and context pointer
that will be passed to your Notify function when you do the
CreateEvent().
