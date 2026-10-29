# UEFI Capsule on Disk Introducation

## Introduction

Firmware update, which aims to fixing system security vulnerabilities, supporting new devices as well as adding new
features, is a common activity in modern computer service. UEFI also provides standard methodology to deliver update
image to firmware Root of Trust for Update (RTU). The entire design follows NIST 800-193 which gives generic principles
and mechanisms for firmware update. Design uses data object named Capsule to carry firmware update information including
update image, update driver and signature.
A typical firmware RTU only services in boot time. Therefore, delivering capsule to RTU requires system reset to give
control back. UEFI spec describes 2 different ways in order to carry Capsule back to RTU – Capsule-in-RAM and
Capsule-on-Disk. The following content covers detailed analysis towards these 2 features.

## Related Concept

Boot trust regions is the similar concept of Trust Controlling Base (TCB) defined in TCG spec, which is considered as
the collection of system resources (hardware and software) that is responsible for maintaining the security policy of
the system.
![Figure 1](../../images/capsule-on-disk-trust-region.jpg)

## Implementation Details

### Capsule-in-RAM

Capsules are delivered from CapsuleUpdate() runtime service and saved in memory when passing control to RTU, which
requires memory content must be persistent across reset. After getting control, RTU module – CapsulePei is responsible
for reassembling capsule image and create capsule HOBs for coming standard capsule authentication and processing module
to consume.
![Figure 2](../../images/capsule-on-disk-capsule-in-ram.jpg)

### Capsule-on-Disk

In Capsule-on-Disk case, capsules are firstly saved as file in EFI system partition on massive storage device which are
identified by UEFI BOOT variables. To tradeoff platform requirement and security concerns, 2 different solutions are
provided in order to deliver capsule to RTU – Solution A) Load Capsule-on-Disk image in memory and rely on capsule in
RAM to deliver to RTU; Solution B) Keep Capsule-on-Disk images in external storage and relying on storage stack in PEI
to load capsule file from disk, then pass to RTU.

#### Solution A(Recommended)

Solution A relies on Capsule-in-RAM. After Capsule-on-Disk images are detected, images are loaded to memory and passed
as Capsule in RAM. Platform must provide memory persistent reset flow to give control back to RTU while content intact.
![Figure 3](../../images/capsule-on-disk-solution-a.jpg)

#### Solution B

Comparing with solution A, Solution B keeps Capsule images in massive storage devices which are naturally persistent
during any types of reset. It eliminates the memory persistent reset requirement in Solution A. After firmware gets
control after reset, it reuses PEI storage stack to load capsule image in memory and pass to later standard capsule
authentication and processing module.
![Figure 4](../../images/capsule-on-disk-solution-b.jpg)

## Security Analysis

The way to deliver capsule data and corresponding parsing logic is very sensitive to platform security as flash is
usually open to write at that moment. If not properly handled, flash could be tampered by adversaries, causing escalate
of privilege, permeant deny of service etc. The design in each solution is different, while security objective, threats
and mitigation plan of each solution also varies a lot.

1) Capsule-in-RAM stores capsules in memory which is described a scatter list. CapsulePei inside BIOS TCB must carefully
validate the scatter list before further processing. During validation, there is only memory access which is inside TCB.
2) Capsule-on-Disk solution A) reuses Capsule-in-RAM security plan without adding extra attack surface and external
input.
3) Capsule-on-Disk solution B) use an intermediate temp relocation file to exclude standard partition and file system
driver from TCB. The temp file and indicator are considered to be new attack surface. Reading temp file though PEI
storage stack is also considered risk to external HW adversary such as DMA attack. Platform is highly recommended to
enable VT-D (IOMMU) if massive storage device requires use of DMA to read temp file

## Platform Consideration

Platform enabling must take security analysis into seriously consideration when enabling firmware update feature. The
attack surface of Capsule-in-RAM is considered relatively simple and small. It is highly recommended to be enabled if
memory persistent reset is available and all update critical device can function normally under this reset on this
platform. If platform also want to support Capsule on Disk, it is highly recommended to choose Solution A because it
doesn’t introduce any new attack surface.
In cases that memory persistent reset is not available or update critical device can’t function normally. Platform can
only use Capsule on Disk Solution B), which can be supported by system normal restart. But platform is highly
recommended to identify security vulnerabilities brought in by this feature and apply proper mitigation plan such as
enabling IOMMU protection.
