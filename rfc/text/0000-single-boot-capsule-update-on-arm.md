# RFC: Enabling Single-Reboot Capsule Firmware Updates on Arm Platforms.

## Metadata

- **RFC Number**: TBD
- **Title**: Enabling Single-Reboot Capsule Firmware Updates on Arm Platforms.
- **Status**: Draft
- **Author**: Yeoreum Yun (`@LeviYeoReum`)

## Change Log

- 2026-06-16: Initial RFC created

## Motivation

Capsule-based firmware update support on Arm platforms is currently limited to
the Capsule-On-Disk delivery mechanism defined in the UEFI specification section [8.5.5. Delivery of Capsules via file on Mass Storage Device](https://uefi.org/specs/UEFI/2.11/08_Services_Runtime_Services.html#delivery-of-capsules-via-file-on-mass-storage-device). While this mechanism is functional, it typically requires multiple reboots to
complete a firmware update.

The UEFI specification also defines the UpdateCapsule() runtime service in section [8.5.3.1. UpdateCapsule()](https://uefi.org/specs/UEFI/2.11/08_Services_Runtime_Services.html#updatecapsule).
This interface allows an operating system to pass capsule images directly to 
firmware at runtime. Capsule processing behaviour is controlled, in part, by 
the CAPSULE_FLAGS_PERSIST_ACROSS_RESET flag in the EFI_CAPSULE_HEADER.

When CAPSULE_FLAGS_PERSIST_ACROSS_RESET is set, the firmware update flow requires two reboots:
1. The operating system submits the capsule to firmware, and 
   the firmware requests a reboot.
2. On the next boot, firmware reconstructs and processes the capsule, 
   performs the flash update, and then triggers another reboot.
3. The platform finally boots using the updated firmware image.

In contrast, when CAPSULE_FLAGS_PERSIST_ACROSS_RESET is not set, capsule processing
may begin immediately at runtime. In this model, the firmware image can be written to 
flash before reset, allowing the platform to reboot directly into the updated firmware image.
This enables a single-reboot update flow, reducing update latency and platform downtime.

However, current Arm platform implementations generally lack a standardized architecture for
supporting this single-reboot runtime firmware update flow.

The [Arm Platform Security Firmware Update for the A-profile Arm Architecture, version 1.0 A-EAC1, DEN0118](https://developer.arm.com/documentation/den0118/latest) specification defines a standard firmware update architecture for
Arm A-profile systems that enables coordinated runtime firmware update handling between
the non-secure software stack and platform security firmware.

This RFC proposes leveraging the Arm Platform Security Firmware Update architecture to
enable single-reboot firmware updates on Arm platforms through the UpdateCapsule() runtime interface.

Enabling this capability provides several benefits and unlocks important use cases, including:
- Reduced firmware update downtime by eliminating the second reboot.
- Faster firmware deployment in cloud and datacenter environments.
- Improved platform availability.
- Better alignment with modern firmware update frameworks that implement
  the Arm Platform Security Firmware Update architecture.

## Technology Background

### 1. Overview of Arm PSA FWU

The [Arm Platform Security Firmware Update for the A-profile Arm Architecture,
version 1.0 A-EAC1, DEN0118](https://developer.arm.com/documentation/den0118/latest) specification
defines a standard firmware update architecture for Arm A-profile systems.
It enables coordinated runtime firmware update handling between
the non-secure software stack and platform security firmware.

In EDK II, this architecture is implemented by [FmpDevicePsaFwuLib](https://github.com/tianocore/edk2/tree/master/ArmPkg/Library/FmpDevicePsaFwuLib).

The firmware storage is organised into multiple banks, with only one bank active at a time.
The active bank is used for booting the platform.

When SetImage() is called to perform a firmware update, FmpDevicePsaFwuLib
invokes the PSA FWU interfaces, such as fwu_open(), fwu_write(), and fwu_commit()
to write the new firmware image to an inactive bank.

After the image has been written, the updated bank is selected for
the next boot and the system enters a trial boot stage.

If the platform boots successfully, the new firmware image is accepted.
If the trial boot fails, the Secure World monitor firmware, such as TF-A,
falls back to the previous firmware image and reports the boot failure
through the FWU interfaces.


### 2. How Capsule on Disk works on Arm Platfoms using PSA FWU

The typical capsule update flow on Arm platforms using PSA FWU and fwupd is as follows:

a. fwupd places the capsule in the EFI System Partition, or in another platform-specific capsule delivery directory.<BR>
b. The system reboots into the firmware update UEFI application provided by fwupd, i.e. [fwupd-efi](https://github.com/fwupd/fwupd-efi).<BR>
c. The fwupd-efi UEFI application calls UpdateCapsule() with the capsule image.<BR>
d. FmpDxe.c authenticates the firmware image.<BR>
e. SetTheImageWithStatus() in FmpDevicePsaFwuLib is invoked.<BR>
f. FmpDevicePsaFwuLib updates the firmware image using the PSA FWU interfaces.<BR>
g. The system reboots again to apply and boot from the updated firmware image.<BR>

Here is brief view how it works with the StandaloneMm:
```
        UEFI (Normal world)           |        StandaloneMm (Secure world)
    ----------------------------------|--------------------------------------
                                      |
    +------------------+              |
    |    fwupd apps    |              |
    +------------------+              |
             |                        |                       +-------------------+
             | (UpdateCapsule())      |                  ---->|  FirmwareStorage  |
             v                        |                  |    +-------------------+
    +------------------+              |                  |     (Gpt parted or etc)
    |   FmpDevicePkg   |              |                  |
    +------------------+              |                  |
      |                               |                  |
      |  Authenticate Image and       |        +-------------------+
      |  update the firmware image    |        |  FwsPlatformLib   |
      |  via FmpDevicePswFwuLib       |        +-------------------+
      |                               |                  |
      |                               |                  |
      |                               |                  | Write authenticated Image
      |                               |                  | via FwsPlatformLib
      |                               |                  |
      |                               |                  |
      -> +---------------------+   PSA ABI (FF-A)    +-------------------+
         |  FmpDevicePsaFwuLib |<------------------> |     FwuSmm.c      |
         |   (FmpDeviceLib)    |   PSA Error code    +-------------------+
         +---------------------+
```

In this flow, the firmware update is initiated from the UEFI environment after rebooting
into the fwupd-efi UEFI application. The capsule is passed to UpdateCapsule(), authenticated
by the FMP driver, and then written to an inactive firmware bank using the PSA FWU interfaces
exposed through StandaloneMm.

After the image is written and committed, the updated bank is selected for the next boot.
The platform then reboots again to enter the PSA FWU trial boot stage and
boot from the updated firmware image.


### 3. Linux support for preemptible Runtime Services

Since Linux `commit a5baf582f4c0 ("arm64/efi: Call EFI runtime services without disabling preemption")`,
EFI runtime services on Arm64 are called without disabling preemption.

This means that long-running runtime service calls are less likely to
introduce scheduling delays for other tasks. In other words,
an UpdateCapsule() call is less likely to block unrelated work on
the system while the operating system remains active.

This support makes runtime capsule delivery more practical on Linux-based Arm platforms.
It allows firmware updates to be initiated while the operating system is running,
with reduced impact on overall system responsiveness.

Furthermore, it helps reduce firmware-update-related downtime
by avoiding the reboot traditionally required to launch a dedicated 
UEFI firmware update application, as is needed for Capsule-on-Disk processing.


### 4. Relevant Specifications
The following specifications are relevant to this proposal:
- [Arm Platform Security Firmware Update for the A-profile Arm Architecture, version 1.0 A-EAC1, DEN0118](https://developer.arm.com/documentation/den0118/latest)<BR>
- [UEFI Specificaton - 8.5.3.1. UpdateCapsule()](https://uefi.org/specs/UEFI/2.11/08_Services_Runtime_Services.html#updatecapsule)


## Goals

### 1. Enable Single-Reboot Capsule Firmware Updates on Arm Platform

As described in the Motivation section, the UEFI specification does not 
prohibit firmware updates from being performed through runtime capsule delivery.

Enabling firmware updates at runtime reduces system downtime by avoiding
the reboot that would otherwise be required to launch a dedicated firmware update application.
This allows the firmware image to be written before reset,
so the platform can reboot directly into the updated firmware image.

### 2. Reassess the Design and Address Concerns Raised in the Initial PR

The initial implementation was proposed in PR [#12193](https://github.com/tianocore/edk2/pull/12193). Several concerns were raised during review. This proposal reassesses the design and updates the approach to address those concerns.

**Q1: The user experience is unacceptable bad from production scenarios.**<BR>

A1: In production environments, reducing downtime during firmware updates
can significantly lower operational costs.

Traditionally, firmware updates require two system reboots, which
increases the cost associated with service interruptions during the update process.

With a single-reboot capsule update mechanism, services can
continue running with minimal interruption while the firmware
update is being applied. This approach reduces the downtime
required to deploy new firmware and helps lowerthe overall
cost of firmware maintenance.

Therefore, I believe this enhancement will improve both
the user experience and overall business efficiency.

**Q2: Contention over hardware access is challenging (sharing between fw and os**<BR>

A2: Contention over hardware access depends on the hardware configuration of the target platform.
Some possible scenarios are considered below:

- If a platform provides a dedicated flash device exclusively for firmware images,
  contention with the operating system is unlikely.<BR>
- If the flash device containing the firmware image is also used 
  for other firmware-managed services, and runtime access would introduce
  unacceptable contention, that platform may choose not to enable runtime capsule updates.<BR>
- In configurations where firmware image storage and variable storage
  share the same flash device, contention may occur. However,
   the UEFI specification already requires runtime service execution to be serialized:
  while one runtime service is executing, another runtime service must not be invoked concurrently.
  Therefore, access serialization is already required by the UEFI runtime services model.<BR>

However, the reduction in firmware-update-related downtime may outweigh
the associated cost on platforms where runtime firmware updates are feasible.

This proposal is not intended to be universally enabled across
all Arm platform configurations. Instead, it defines an optional
capability for platforms where runtime firmware updates are practical
and where reducing system downtime is a priority.

**Q3: Runtime isn't an area of UEFI we want to see expanded and if anything, we would want to see reduced.**<BR>

A3: Enabling single-reboot capsule firmware updates does increase runtime memory usage
because components such as FmpDxe and DxeCapsuleFmpLib must support runtime driver functionality.
As a result, additional code and data must remain resident after ExitBootServices().
However, whether this increase in runtime memory usage is acceptable in exchange for
reducing firmware-update-related downtime depends on the platform.
Different platforms have different constraints, deployment models, and availability requirements.

For this reason, this proposal treats runtime capsule update support as an optional platform capability.
Platform implementers can decide whether the runtime memory overhead is justified by
the benefit of eliminating the additional reboot required by Capsule-on-Disk processing.

This is a trade-off that should be left to the platform owner or implementer.

**Q4 Runtime memory management and runtime crypto adds a lot of growth and bloat.**<BR>

A4: Agreed. To address this concern, the updated design discussed in this RFC
avoids runtime memory management. Unlike the previous PR, which allocated and
freed memory at runtime, eliminates the need for dynamic memory allocation
during runtime service execution.

The cryptographic operations required to authenticate the capsule image are
now performed by the Update Agent, implemented as a StandaloneMm application in the Secure World.
This also provides stronger protection against malicious actors in the Normal World,
because capsule authentication is performed in an isolated execution environment rather than 
in the non-secure UEFI runtime context.

**Q5 Runtime memory management is risky because FW is not in control of the system. "Heap management" may work but will likely add to ongoing issues around memory stability and hibernate/resume reliability.**<BR>

A5: The updated design addresses this concern in the same way as described in A4.

The revised implementation does not perform heap allocation or
free memory during runtime service execution.

**Q6 Runtime crypto doesn't actually provide security value which is why isolation (secure world or smm) must be used.**<BR>

A6: Thank you for your suggestion, we have incorporated this in the revised design
which moves image authentication out of the common CheckTheImage() path for
runtime-enabled FmpDxe drivers.
The authentication is now performed in the StandaloneMm side in FwuSmm.c
when fwu_commit() is invoked.

## Requirements

The existing capsule update process and requirements remain unchanged.

Additionally:
  - The operating system must support UEFI Runtime Service calls being interrupted or preempted,
    in accordance with UEFI Runtime Service reentry restrictions.

  - The SPMC must support preemption when receiving Non-secure interrupts.

## UEFI/PI Specification Impact

No impact.

## Backward Compatibility

Not Applicable.

## Platform/Package Impact

### 1. FmpDevicePkg/FmpDxe

In addition to the changes required to support a runtime-enabled FmpDxe driver,
image authentication can no longer be performed in CheckTheImage()
when operating in runtime mode. Therefore, the authentication flow is moved
to the platform-specific implementation through FmpDeviceLib.

According to [Arm Platform Security Firmware Update for the A-profile Arm Architecture, Version 1.0 A-EAC1 (DEN0118)](https://developer.arm.com/documentation/den0118/latest)<BR>,
authentication-related processing may be performed in fwu_commit().
To enable image authentication in the firmware update agent,
this change extends the data passed through SetImageWithStatus().

Previously, only the raw firmware image payload was passed to the firmware update agent.
With a runtime-enabled FmpDxe driver, the associated capsule and
FMP headers are also forwarded, allowing the firmware update agent to
authenticate the complete update image.

### 2. FmpDevicePkg/Library/FmpPayloadHeaderLib

As image authentication is delegated to the firmware update agent,
the agent must parse FMP headers. Therefore, this change converts
FmpPayloadHeaderLib into a BASE library and exposes it for use by firmware update agents.

### 3. MdeModulePkg/Library/DxeCapsuleLibFmp

The relevant code is modified to support runtime-enabled FmpDxe instances.

## Unresolved Questions

None.

## Prior Art/Related Work

 - [edk2 PR#12193](https://github.com/tianocore/edk2/pull/12193)

## Alternatives

None.

## Implementation Design

As described in Platform/Package Impact, FmpDevicePkg requires the following changes:
- Support for a runtime-enabled FmpDxe driver.
- Delegation of image authentication from FmpDxe to the firmware update agent when operating in runtime mode.

According to [Arm Platform Security Firmware Update for the A-profile Arm Architecture, Version 1.0 A-EAC1 (DEN0118)](https://developer.arm.com/documentation/den0118/latest)<BR>,
authentication-related processing may be performed in fwu_commit().
Therefore, image authentication is deferred to SetImageWithStatus(),
where the image is handed off to the firmware update agent.

Since authentication is performed by the firmware update agent,
the complete image, including the FMP headers, must be provided to the agent.

As fwu_commit() can return FWU_AUTH_FAIL, authentication can be performed
within the firmware update agent during the commit phase.

In addition, fwu_commit() supports returning FWU_RESUME, which allows
the firmware update process to relinquish execution to the Normal World and
resume later. The platform-side implementation of the storage interface can
tune storage operations according to hardware capabilities, such as block size.
This helps reduce the amount of time the CPU spends in Secure World.

With these changes, a new FmpDxeRuntime.dsc is introduced, allowing
platforms to support single-reboot firmware updates using the following flow:

1. The OS invokes UpdateCapsule() to submit the capsule image, for example:

   '''cat {CAPSULE} > /dev/efi-capsule-loader'''

2. UpdateCapsule() invokes the runtime version of ProcessFmpCapsuleImage(),
   which subsequently calls FmpDxe::SetTheImage().

3. SetImageWithStatus() in FmpDeviceLib delivers the complete FMP image,
   including the associated headers, to the firmware update agent.
   The firmware update agent then performs image authentication and
   writes the image to firmware storage.

4. Control returns to the OS, and a single reboot is performed to activate
   the updated firmware.

### Architecture Overview

High-level architecture description:

**Single Reboot Capsule update flow**
```
           OS (Normal world)                |        StandaloneMm (Secure world)
    ----------------------------------------|--------------------------------------
                                            |
FmpDxe::SetTheImage()                       |
    +------------------+                    |                            Update Agent
    |  CheckTheImage() |                    |
    +------------------+                    |                        +-------------------+
             |\                             |                        |  FirmwareStoarge  |
             | # Defer Image Authentication |                        +-------------------+
             |   to be performed by update  |                                 |
             |   agent                      |                                 |
             |                              |                                 |
    +------------------+                    |                                 |
    |   SetTheImage()  |                    |                        +-----------------+
    +------------------+                    |                        | FwsPlatformLib  |
             |\                             |                        +-----------------+
             | - The whole Image is         |                                 |\
             |   delivered.                 |                                 | - fwu_write() receives the full Image
             |   (FmpHeader is preserved).  |                                 |   including FmpHeader and is written
             |                              |                                 |    to a buffer.
             |                              |                                 |
             |                              |                                 | - When fwu_commit() is called the image
             |                              |                                 |   in the buffer is authenticated and on
             |                              |                                 |   success is written to the firmware
             |                              |                                 |   update storage area.
             |                              |                                 |
    +---------------------+            PSA ABI (FF-A)                +-------------------+
    |  FmpDevicePsaFwuLib |<---------------------------------------> |     FwuSmm.c      |
    |   (FmpDeviceLib)    |     fwu_open()/write()/commit()          +-------------------+
    +---------------------+
```

### Detailed Design

The single-reboot capsule update flow moves image authentication from
FmpDxe to the firmware update agent running in StandaloneMm.

When FmpDxe::SetTheImage() is invoked, CheckTheImage() no longer authenticates
the image directly. Instead, authentication is deferred to the Secure World update agent.
SetTheImage() then passes the complete firmware image, including the preserved FMP header,
through FmpDevicePsaFwuLib using the PSA Firmware Update FF-A interface.

On the Secure World side, FwuSmm.c receives the image via fwu_open(), fwu_write(),
and fwu_commit(). The full image is first written to a staging buffer.
When fwu_commit() is called, the update agent authenticates the buffered image.
If authentication succeeds, the image is written to the firmware update storage
through FwsPlatformLib and FirmwareStorage.

This design enables capsule updates to be completed before reboot,
allowing the updated firmware to be activated with a single reboot.

### Code Examples

A pull request will follow after the initial RFC discussion.

## Testing Strategy

Validate single-reboot capsule firmware updates on Arm platforms using
the Linux /dev/efi-capsule-loader interface.

Submit a capsule image, for example:

''' cat {capsule} > /dev/efi-capsule-loader '''

Then verify that:
- The updated firmware image is applied successfully after reboot.

## Migration/Adoption Plan

Not applicable.

## Guide-Level Explanation

### For Package Developers
None.

### For Platform Developers

Platform developers must update their platform-specific FmpDeviceLib implementation
to support runtime operation.

The FmpDeviceLib implementation is responsible for:
- Handling image delivery from a runtime-enabled FmpDxe instance.
- Supporting deferred image authentication by forwarding the complete image,
  including FMP headers, to the firmware update agent.
- Communicating with the firmware update agent through the platform firmware update interface,
  such as PSA Firmware Update over FF-A.
- Ensuring that the runtime update path does not rely on boot-services-only
  functionality.
- Mapping firmware update agent errors, such as authentication failures,
  to appropriate capsule update status reporting.

Platforms should also ensure that the Secure World firmware update agent can stage,
authenticate, and commit the complete firmware image during the update flow.

### For End Users (if applicable)

End users do not need to manually perform a multi-reboot firmware update sequence.

The firmware update can be initiated from the operating system using
the standard capsule update mechanism.
After the capsule is submitted, the firmware image is staged and authenticated before reboot.
The updated firmware is then activated with a single reboot.
