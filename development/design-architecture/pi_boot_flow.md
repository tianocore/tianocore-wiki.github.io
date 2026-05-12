# Platform Initialization (PI) Boot Flow

[&lt;img](https://raw.githubusercontent.com/tianocore/tianocore.github.io/master/images/PI_Boot_Phases.JPG)src="<https://raw.githubusercontent.com/tianocore/tianocore.github.io/master/images/PI_Boot_Phases-tn.jpg>&gt;

**Security (SEC) Phase**
The Security ([SEC](../../reference/faqs-glossaries/acronyms_and_glossary.md)) phase
is the first phase in the PI Architecture architecture and is
responsible for the following:
\* Handling all platform restart events

- Creating a temporary memory store
- Serving as the root of trust in the system
- Passing handoff information to the PEI Core

In addition to the minimum architecturally required handoff information,
the SEC phase can pass optional information to the PEI Core, such as the
SEC Platform Information PPI or information about the health of the
processor.
[UEFI.org -PI VOLUME 1: Platform Initialization Specification 1.2
Pre-EFI Initialization Core
Interface](https://uefi.org/specsandtesttools)

**Pre-EFI Initialization (PEI) Phase**
The Pre-EFI Initialization
([PEI](../../reference/faqs-glossaries/pei.md)) phase of the PI
Architecture specifications (hereafter referred to as the “PI
Architecture”) is invoked quite early in the boot flow. Specifically,
after some preliminary processing in the Security (SEC) phase, any
machine restart event will invoke the PEI phase.
The PEI phase will initially operate with the platform in a nascent
state, leveraging only onprocessor resources, such as the processor
cache as a call stack, to dispatch Pre-EFI Initialization Modules
(PEIMs).
These PEIMs are responsible for the following:
\* Initializing some permanent memory complement

- Describing the memory in Hand-Off Blocks (HOBs)
- Describing the firmware volume locations in HOBs
- Passing control into the Driver Execution Environment (DXE) phase

Philosophically, the PEI phase is intended to be the thinnest amount of
code to achieve the ends listed above. As such, any more sophisticated
algorithms or processing should be deferred to the DXE phase of
execution.
The PEI phase is also responsible for crisis recovery and resuming from
the S3 sleep state. For crisis recovery, the PEI phase should reside in
some small, fault-tolerant block of the firmware store. As a result, it
is imperative to keep the footprint of the PEI phase as small as
possible. In addition, for a successful
[ACPI S3](../../reference/specs-standards/pi.md) resume,
the speed of the resume is of utmost importance, so the code path
through the firmware should be minimized. These two boot flows also
speak to the need to keep the processing and code paths in the PEI phase
to a minimum.
The implementation of the PEI phase is more dependent on the processor
architecture than any other phase. In particular, the more resources the
processor provides at its initial or near initial state, the richer the
interface between the PEI Foundation and PEIMs. As such, there are
several parts of the following discussion that note requirements on the
architecture but are otherwise left architecturally dependent.
[UEFI.org -PI VOLUME 1: Platform Initialization Specification 1.2
Pre-EFI Initialization Core
Interface](https://www.uefi.org/specsandtesttools)

**Driver eXecution Environment (DXE) Phase**
The Driver Execution Environment
([DXE](../../governance/project-management/tasks_ueficpupkg_cpudxe_mp_support.md)) phase is where most
of the system initialization is performed. Pre-EFI Initialization (PEI),
the phase prior to DXE, is responsible for initializing permanent memory
in the platform so that the DXE phase can be loaded and executed. The
state of the system at the end of the PEI phase is passed to the DXE
phase through a list of position-independent data structures called
Hand-Off Blocks (HOBs). HOBs are described in detail in the Platform
Initialization Hand-Off Block Specification.
There are several components in the DXE phase:
\* “DXE Foundation”

- “DXE Dispatcher”
- A set of “DXE Drivers”

The DXE Core produces a set of Boot Services, Runtime Services, and DXE
Services. The DXE Dispatcher is responsible for discovering and
executing DXE drivers in the correct order. The DXE drivers are
responsible for initializing the processor, chipset, and platform
components as well as providing software abstractions for system
services, console devices, and boot devices. These components work
together to initialize the platform and provide the services required to
boot an operating system. The DXE phase and Boot Device Selection (BDS)
phases work together to establish consoles and attempt the booting of
operating systems. The DXE phase is terminated when an operating system
is successfully booted. The Dxe Core is composed of boot services code,
so no code from the Dxe Core itself is allowed to persist into the OS
runtime environment. Only the runtime data structures allocated by the
Dxe Core and services and data structured produced by runtime DXE
drivers are allowed to persist into the OS runtime environment.
[UEFI.org -PI VOLUME 2: Platform Initialization Specification 1.2 Driver
Execution Environment Core
Interface](https://www.uefi.org/specsandtesttools)

**Boot Device Selection (BDS) Phase**
The Boot Manager in DXE executes after all the DXE drivers whose
dependencies have been satisfied have been dispatched by the DXE
Dispatcher. At that time, control is handed to the Boot Device Selection
([BDS](../../platforms-packages/core-packages/armplatformpkg_bds.md)) phase of execution.
The BDS phase is responsible for implementing the platform boot policy.
System firmware that is compliant with this specification must implement
the boot policy specified in the Boot Manager chapter of the UEFI 2.0
specification. This boot policy provides flexibility that allows system
vendors to customize the user experience during this phase of
execution.
The Boot Manager must also support booting from a short-form device path
that starts with the first node being a firmware volume device path. The
boot manager must use the GUID in the firmware volume device node to
match it to a firmware volume in the system. The GUID in the firmware
volume device path is compared with the firmware volume name GUID. If a
match is made, then the firmware volume device path can be appended to
the device path of the matching firmware volume and normal boot behavior
can then be used.
The BDS phase is implemented as part of the BDS Architectural Protocol.
The DXE Foundation will hand control to the BDS Architectural Protocol
after all of the DXE drivers whose dependencies have been satisfied have
been loaded and executed by the DXE Dispatcher.
The BDS phase is responsible for the following:
\* Initializing console devices

- Loading device drivers
- Attempting to load and execute boot selections

If the BDS phase cannot make forward progress, it will reinvoke the DXE
Dispatcher to see if the dependencies of any additional DXE drivers have
been satisfied since the last time the DXE Dispatcher was invoked.
[UEFI.org -PI VOLUME 2: Platform Initialization Specification 1.2 Driver
Execution Environment Core
Interface](https://www.uefi.org/specsandtesttools)

**Transient System Load (TSL) Phase**
[TSL](../../reference/faqs-glossaries/acronyms_and_glossary.md) is the 1st stage of
the boot process where the OS loader is an EFI application.

**Run Time (RT) Phase**
UEFI has a limited set of Runtime
([RT](../../reference/faqs-glossaries/acronyms_and_glossary.md)) Services that are
available after the Operating System boots and takes over the system.
The primary runtime services enable the OS to read and write environment
variables. There are also services that abstract the Real Time Clock,
produce an monotonically increasing count, and support updates of
firmware components via capsules.
It is legal for OS to call UEFI runtime services using virtual
addressing, so some of the runtime services are involved in making sure
the code is callable from a given virtual address space. Making a
virtual call is a one way gate, and the OS must request virtual calling
when it boots and provide the firmware with the virtual mappings for the
physical addresses required by the runtime firmware. The firmware then
fixes it self up so that the next call can be made from the virtual
address space. Due to this complexity of managing virtual calling
runtime code in UEFI is usually isolated into runtime only drivers.

**After Life (AF) Phase**
After the Operating System takes over from the BDS via its boot (TSL),
only a small set of UEFI is left behind. If the hardware or OS crashes,
firmware could try and implement some form of recovery or remediation
action. This is the After life. UEFI and PI do not spec any required
behavior in this phase.

## Glossary

See [
Acronyms](../../reference/faqs-glossaries/acronyms_and_glossary.md) for glossary and acronym information.
