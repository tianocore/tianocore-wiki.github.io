# EDK II FAQ

**EDK II FAQ**

[Differences between EDK and
EDK II](differences_between_edk_and_edk_ii.md)

[Differences between
UDK2010 and EDK II](differences_between_udk2010_and_edk_ii.md)

[Build](../../build-tooling/build-workflows/build.md) questions

[Debug FAQ](debug_faq.md)
questions

[DEC FAQ](dec_faq.md) questions

[ECP](../external-resources/ecp.md) EFI Compatibility
package questions

[Execution order FAQ](execution_order_faq.md) questions

[GUID FAQ](guid_faq.md)
questions

[NT32](../../archives/platforms-packages/nt32_pkg.md) questions

[PCD](pcd.md) questions

[Shell](../../platforms-packages/component-guides/shell.md) questions

[Virtualization FAQ](virtualization_faq.md) questions

[UEFI application](../../development/tutorials-howto/uefi_application.md) questions

[UEFI
Drivers](../specs-standards/uefi.md) questions

[USB stack](../../platforms-packages/component-guides/usb_stack.md)
questions

### Is an a priori file checked first?

Yes

### Is DXE single threaded now as PEI is?

Yes, could be multithreaded but the current EDK II implementation is
single threaded. However, It is possible to use the MP Services Protocol
in the DXE Phase to execute code on more than one processor. The MP
Services Protocol is typically used by the CPU initialization code to
propagate settings to all logical processors in the platform. The
UEFI/PI services, Protocols, and PPIs are not designed with threading in
mind. As a result, even though the MP Services Protocol provides a
capability to run code on another logical processor, the code that is
executed must be very carefully designed to not use any of the UEFI Boot
Services, UEFI Runtime Services, or Protocols services that are not
explicitly documented to be MP safe. In addition, there are many library
services that make use of underlying UEFI Boot Services, UEFI Runtime
Services, or Protocols, so many of the library services cannot be used
from code running on a logical processor other than the BSP.

### Is SMI handled by BIOS or OS BIOS?

The OS does not see SMI.

### Is there a type Intel Development Environment IDE or are there Visual Studio solution files?

There was a VS .sol for EDK 1, no plan for EDK II.

### What specifies flash organization?

The flash layout is defined in the .FDF file for the platform

### Why does EDK II use library classes? Are there different instances?

EDK II’s implementation uses library classes as a way of tying different
implementations to the same library name. This will resolve reference
and make it so the Firmware developer can map the implementations at
different sections of the Boot sequence. This is done because the
implementation may differ for PEI vs DXE or DXE vs Runtime, etc.

The primary benefit is that code can be highly portable and reusable
between different phases. For example, code written originally for DXE
can be rebuilt in PEI without requiring modification. There are
limitations, but there are far fewer differences visible to a programmer
if libraries are used properly.

### Is there an example of VFR?

Yes, there is an example in EDKII in
MdeModulePkg\Universal\DriverSampleDxe. The sample Vfr.vfr describes a
sample forms, check boxes, action buttons, and etc. that can be drawn in
HII

### When Recovery runs what part of the firmware gets updated on the UEFI target system?

Recovery is invoked during PEI phase and it will look for an image on
some media, typically USB thumb drive for the file whose name is defined
in the .fdf file.”. The .fdf file defines the flash layout and each
platform gets to define what parts of the FLASH device are updatable or
not. For example, the recovery code will update the main part of the
flash device for what might be defined as “FVMAIN” in the .Fdf file with
this image. The other sections of the flash device would remain intact.

### What do the Version numbers mean for Package and platforms in the build description files for EDK II?

A package is identified by a combination of the PACKAGE_GUID and
PACKAGE_VERSION. Every time a new version of a package is released, the
PACKAGE_VERSION must be set to a higher value. The package owner gets to
decide when the major or minor parts of the version value are increased.
If a non backward compatible change is made to a package, then a new
PACKAGE_GUID must be generated, and the PACKAGE_VERSION values can be
restarted with a low value determined by the package owner.

### What does EDK II Flash mapping tool do?

The flash mapping tool will get platform, flash and module information
from target.txt,platform.dsc, flashmap.fdf, package.dec and module.inf.
it will generate Ffs, Fv, FD and Section data depending on command line
options. The tool will generate a FV directory which includes a FFS
directory, Fv file, Fv INf file and Fd file. The FFS directory contents
all modules directory which contents Ffs files and section files. All
output of Flash tool output will be in FV directory.

### How do I convert non-EFI oproms or Legacy ROM to UEFI oproms?

There are no tools to convert a Legacy Option ROM in source/binary form
to a UEFI Driver. There is no such thing as a “UEFI oprom”. There are
PCI Option ROMs, and the PCI Specification defines the binary layout of
a PCI Option ROM that may contains multiple images. These can be Legacy
Option ROM images, UEFI Drivers, and many other image types. The EfiRom
utility simply follows the PCI Specification for the binary layout of
PCI Option ROMs and provides the ability to put multiple images into a
single PCI Option ROM. The image types supported by the tool are Legacy
Option ROM images and UEFI Drivers.

### What about the libraries with EDK II?

EDK II makes use of libraries to allow different implementations for the
same functionality. EDKII introduces the concept of Library
Class/Library Instance. Library class is a set of standard interfaces
for common support routines, and library instance supplies the
implementation of these interfaces. Platform integrator can select
different implementations upon various application scenarios.

### Does EDK II support ASL code?

EDK II has full support for ASL compilers from Microsoft and ACPICA. The
EDK II has include files for ACPI Specifications up through ACPI 4.0.
These include files are in /MdePkg/Include/IndustryStandard/Acpi.\*. The
UDK 2010 supports a number of reference platforms including Lakeport,
Stoutland, and Richford. All of these platforms have source modules for
the ACPI Tables and ASL code required for these platforms to boot ACPI
complaint operation systems.

### Does EDK II address Legacy USB?

No, BIOS vendors provide that

### What does the library code call?

Library functions are either full implementations of the library
function with no additional calls, or a library function may call other
library functions or services. It is up to the library implementation to
decide if it needs to call another library or service or not.

### What is recommended for compiling drivers and applications with 64bit?

Since the build requires the use of at least one .DSC file the OVMF
package could be used as a platform package to insert a driver or
applications for building. Then to just build that driver or platform if
you invoke the build in the directory where your driver or application
resides, then just your driver or application gets built.

Add the path to your Drivers and Applications INF files to your DSC file
in the \[Components.X64\] section. When the build is started for that
DSC file, the newly added Drivers and Applications will be built for X64
and the generated .EFI files will be placed in the build output
directory. If you want the Drivers and Application placed in a FLASH/ROM
image, then the paths to the INF files will also have to be added to
your FDF file.

### For Lib classes (E.G. debuglib) how do you know which packages go with which library class?

Search INF Libclass = . . .

### In the FDF what is the list of order for final image?

Typically the order listed is order of appearance

### How do you create additional FDF segments?

The FDF Spec should have information on specifying in the FDF for a
platform. See [EDK II Specifications](../specs-standards/edk_ii_specifications.md)

### Any formal forum?

Yes, Join the Sourceforge and sign up for the developer for EDK II.
[https://sourceforge.net/apps/mediawiki/tianocore/index.php?title=Mailing_Lists](https://sourceforge.net/apps/mediawiki/tianocore/index.php?title=Mailing_Lists)
and subscribe to the mailing lists.

### What happens when there is no more temporary RAM?

Running out of temp RAM could cause problems

The results are not predictable. Temporary RAM is used for both the
Stack and for storing HOBs. These two regions grow towards each other.
If they overlap, then the most likely scenario is that the Stack will
corrupt HOBs, and on a build with DEBUG() and ASSERT() messages enabled
the next time the HOB list is accessed, an ASSERT() will fire.

### What is the Macro CPU breakpoint?

CPU breakpoint –comes in as code to cause the processor to halt at that
point.

The EDK II does not have a macro to generate a software breakpoint.
Instead, the BaseLib in the MdePkg has a function called
CpuBreakpoint(). When this function is called a SW breakpoint is
generated. On IA32/X64 platforms, this is an INT 3.

### Can we use the same serial port for FW debug sessions and for an OS channel session?

No. You can’t use same serial port for Firmware debug session & os
channel session

### Is there a prescribed method for adding an embedded legacy option ROM to be loaded for legacy boot?

The code to support this is part of the file PciPlatform.c found in the
platformX example in the directory: R8platformX
Pkg\Platform\IntelXpg\platformX\PciPlatform\Dxe

Basically, the Option ROM is included in the table (mPciOptionRomTable)
where each embedded ROM is defined by: File Guid, and PCI Vendor and
Device ID.

In the Procedure:

`EFI_STATUS`
`EFIAPI`
`GetPciRom (`
`IN  EFI_PCI_PLATFORM_PROTOCOL         *This,`
`IN  EFI_HANDLE                        PciHandle,`
`OUT VOID                              **RomImage,`
`OUT UINTN                             *RomSize`
`)`

A found device is checked against the table, and if there is an Option
Rom available, a pointer to the Rom image is provided for load.

### Is there documentation or a guide on the IA32FamilyCpuPkg as far as what needs to be done for the different
processor support?

There is not a lot of document but the one to modify the most is the
CpuMpDxe driver

### Is there any other Native EDK II platform available?

DUET, [OVMF](https://www.tianocore.org/ovmf/)

### Is there any porting guide available?

There is training material that has a high level list of the porting the
different phases but there is no EDK I to EDK II porting guide.

### How does the Intel® BLDK interact / interface with EDK II?

Intel® BLDK has EDKII within it. The biggest technical difference
between Intel® BLDK and EDK II are that some of the options in the
Intel® BLDK are routed to the VPD area of the platform which further
enables binary editing. Some of the same options are also part of EDK II
in that they are extracted by the PCDs but where they are stored may be
different.

### Is there a template for the BSF file but there is not a way to create one from scratch?

No, most take old BSF and then port it to a new platform. There isn’t a
tool in existence to do one from scratch.

### What is the biggest difference in the Intel ® BLDK vs. EDK II?

The biggest difference has to do with binary editing ability. Also
reduced functionally (e.g. Intel® BLDK will not have CSM support )

### What does “EFI API” mean and why?

A\) It is used for function prototypes and declaration. It is for the
source style for EDK II.It means that this function follows the calling
convention as described by chapter 2 of the UEFI spec. Calling
convention described for IA32, X64, IPF, and ARM. This forces the
compiler to follow that exact calling convention for Boot services,
Runtime services, also for Protocols and PPIs need to have EFI API.
Internal worker functions do not need the EFI API.

B\) MSFT vs UEFI calling conventions are similar so they would work.

### Why do we have to put GUIDs and PPI, Libraries have to be under the /Include directory

There is no assumption from the build tools on where things are located
but for design and consistency there is a coding style of putting them
under the /Include directory.

### Can a Package have multiple token spaces? And can they be shared across multiple packages? For instance is it
possible to have 5 tokens in 1 package and 5 in another?

a\) Yes, packages can have multiple token spaces. b) Because token space
is a GUID with a name, and also a token (32 bit ) associated with it. It
is assumed to only be part of one package. We would want to avoid
splitting tokens across multiple packages because of coordination
issues.

### What are the different Packages in EDK II and what are their inner dependences. Can you give a brief description of
the packages?

There it is supposed to be DEC has file header that has a description
about the package and what the package is. We do Doxygen for DEC which
will create a CHM and a description of the package. Inf has the
description of modules and what each module does (what it consumes what
it produces) EDK II may not have all the documentation and descriptions
may be lacking. The plan is to improve over this over time.

### Is there an example for read memory and or Memory mapped IO functions? Are there 2 separate calls for 32 and 64 bit?

There is an IO Library that supports both port IO and memory mapped IO
and you give the exact size that you want to do the access (e.g. 8 bit,
16 bit, 32, etc). There are also a number of helper functions (ORing,
masking,..) There is also a PCI library for doing PCI access.

### Is there any EDK I code in the EDK II firmware?

No, There can be EDK I with the Edk Compatibility Package( ECP). EDK II
can have EDK I if the ECP is used. ECP allows the building of EDK II
with EDK I modules in the EDK II build base. ECP allows for EDK I to be
brought up to the Latest UEFI Specs 2.3 and PI 1.2 . ECP does the
translations from specs.

### What is the meaning of the different status EFI Status vs other Status code?

EFI Status is related to the UEFI Spec “EFI Services” which is part of
the DXE phase of the boot flow. One of the goals with the libraries was
that there is a specific class of libraries that has a module type is
“Base”. What is meant by “Base” is that library is compatible everywhere
(SEC, PEI, DXE, SMM, etc.).To do this it cannot make any EFI assumptions
so a subset of the EFI status codes was chosen for “return Status”.

### Is there a doc template for listing dependencies?

Yes, – put this in the Module .INF dependences

### Is there a good example of .Inf.?

MdeModulePkg\Application\HelloWorld\HelloWorld.inf

### Is there a good example in the mde pkg?

MdeModulePkg\Application\HelloWorld\HelloWorld.inf

### What is there in EDK II relating to the FV and COU microcode patch?

Not a way to just change the patch. It has to be part of the build.
There is nothing to patch the .FD location. It needs to update the .fdf
file. There has been a request for a tool to do this.

### Is there a tool that can operate on a binary image?

The only type of information that can be patched in a binary image is of
type PCD VPD (vital product data) that go into an uncompressed section
of the binary/ROM image as defined in the .FDF file. Post Build. The
only other type is Patch able in module that allows you to adjust values
within a PECOF image within a PEIM or DXE driver. During build

### Is the Dispatch order of modules recorded?

The report status code is the correct method. But this is not in place
for the a priori files. To work around this, just put minimum to get the
report status code available and working.

`-The order will be with the DXE dispatcher. The order of the protocols will be the same as by using “dh” from the shell`
`-This is not available with PEI.`
`Suggestion is adding the dispatch order to the end of the Pei and Dxe. It reports status of the exact order`

### Since a lot of boards no longer have a serial port can AMT be used for console redirection port?

This is possible but since AMT shows up as PCI device there are issues
with serial on PCI because it can init early but the resources might not
be the same when the PCI Bus driver enumerates so this method may lose
connections.

### How do you assign resource?

We are currently working on another library for covering a generic PCI
device to manage the redirection port this.

### What are the Core version to match which spec versions of EDK(s)?

EDK II is at the spec level: UEFI 2.3 / PI 1.2 Spec implementation – EDK
II builds can use: EDK Compatibility Package (ECP) – with EDK 1117 (EDK
1.01) that are at the spec level: UEFI 2.0 and Intel Framework 0.9

### How do I burn a CD that is bootable (IA32.ISO)?

The easiest way to make a fedora uefi cdrom/dvdrom is to use their
boot.iso install image. It is buried on their site though Simply burn
the boot.iso to a cdrom or dvdrom and boot from your uefi system (in the
boot manager select EFI DVDROM/CDROM boot)

The following location is present on all mirrors (IE at univ of Oregon)
boot.iso Should also see the efidisk.img and efiboot.img in the same
directory.
[https://mirror.uoregon.edu/fedora/linux/development/14/x86_64/os/images/](https://mirror.uoregon.edu/fedora/linux/development/14/x86_64/os/images/)

or

[https://ftp.linux.ncsu.edu/pub/fedora/linux/releases/14/Fedora/x86_64/os/images/](https://ftp.linux.ncsu.edu/pub/fedora/linux/releases/14/Fedora/x86_64/os/images/)

so your favorite mirror /fedora/linux/development/14/x86_64/os/images
then look for boot.iso and burn it with your burner software. If you
want to make a usb stick fedora EFI bootable or want to do a netboot
install see below. Minimal boot image method from fedora 14 docs.

[https://docs.fedoraproject.org/en-US/Fedora/14/html/Installation_Guide/Making_Minimal_Boot_Media.html](https://docs.fedoraproject.org/en-US/Fedora/14/html/Installation_Guide/Making_Minimal_Boot_Media.html)
On the web there are 3 methods to install UEFI x64 fedora 14 to usb
sticks:

[https://blog.fpmurphy.com/2010/10/methods-to-uefi-install-fedora-14.html](https://blog.fpmurphy.com/2010/10/methods-to-uefi-install-fedora-14.html)

For Ubuntu goto their cdimage to get the .iso images
[https://cdimage.ubuntu.com/](https://cdimage.ubuntu.com/)

desktop would be maverick release

[https://cdimage.ubuntu.com/releases/10.04.1/release/](https://cdimage.ubuntu.com/releases/10.04.1/release/)

ubuntu-10.04-dvd-amd64.iso 29-Apr-2010 15:28 4.1G Install/live DVD for
64-bit PC (AMD64) computers (standard download)
