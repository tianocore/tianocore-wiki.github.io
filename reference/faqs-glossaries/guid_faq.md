# GUID FAQ

Frequently asked questions about GUIDs

### How are GUIDs generated?

Globally” Unique Identity 128-bit quantity defined by Wired for
Management WfM 2.0 specification on Intel Web site. There is a Web site
where a GUID can be generated: [https://www.guidgen.com](https://www.guidgen.com) Also, Microsoft
Visual Studio also has a GUID generator. These unique identifies will
and if you look at the wiki on this, the reality is that the probability
of a collision is very, very, very small. From RFC 4122:

### How do you decode all the GUIDs displayed on the console?

The output file Guid.xref gets generated as part of the build. This file
will be located in the build output directory. This file will list all
the GUIDs and the modules that are associated for each GUID Another way
to decode GUIDs is to use a search utility on your WORKSPACE, and search
only the .DEC and .INF files in your WORKSPACE. The DEC files contain
declarations of all Protocols, PPIs, and GUIDs with a mapping between
the GUID value and the C name for the global variable that is used from
C sources. The INF files contain FILE_GUID defines for the GUID name of
the FFS file that is stored in an FV.

DEC Examples \[Guids\]

`## Guid specify the device is the console out device.`
`#  Include/Guid/ConsoleOutDevice.h`
`gEfiConsoleOutDeviceGuid       = { 0xD3B36F2C, 0xD551, 0x11D4, { 0x9A, 0x46, 0x00, 0x90, 0x27, 0x3F, 0xC1, 0x4D }}`

\[Protocols\]

`## Load File protocol provides capability to load and unload EFI image into memory and execute it.`
`#  Include/Protocol/LoadPe32Image.h`
`#  This protocol is deprecated. Native EDKII module should NOT use this protocol to load/unload image.`
`#  If developer need implement such functionality, they should use BasePeCoffLib.`
`gEfiLoadPeImageProtocolGuid    = { 0x5CB5C776, 0x60D5, 0x45EE, { 0x88, 0x3C, 0x45, 0x27, 0x08, 0xCD, 0x74, 0x3F }}`

INF Example

\[Defines\]

`INF_VERSION                    = 1.25`
`BASE_NAME                      = DiskIoDxe`
`FILE_GUID                      = 6B38F7B4-AD98-40e9-9093-ACA2B5A253C4`
`MODULE_TYPE                    = UEFI_DRIVER`
`VERSION_STRING                 = 1.0`
`ENTRY_POINT                    = InitializeDiskIo`

### Is there an easy way to decode the GUIDs including customer defined GUIDS and GUIDs defined in the Specs?

Request for a tool to manage this. Currently there is no tool to do
this. A work around would be to use the Guid.xref file and import it to
a spread sheet and compare to the GUIDs in the spec.

GUIDs used a Protocols, PPIs, and GUIDs in source code must be declared
in a DEC file, and the DEC file supports comment blocks for GUIDs, so
brief descriptions of the Protocol, PPI, and GUID are available. All the
GUIDs from the Industry Standard Specifications are in the MdePkg, and
are also in the Doxygen generated documentation for the MdePkg. The full
.h file for each Protocol/PPI/GUID must be in the same package as the
DEC file that declares the Protocol/PPI/GUID. Packages that are part of
the UDK 2010 follow a recommended practice for the include file layout,
so .h files associated with Protocols/PPIs/GUIDs can be easily found:

```\`
```.dec`
`Include\`
`Protocol\`
`<Protocol1.h>`
`<Protocol2.h>`
`Ppi\`
`<Ppi1.h>`
`<Ppi2.h>`
`Guid\`
`<Guid1.h>`
`<Guid2.h>`

### Is there a tool to track the chain of GUID to Protocol driver?

The build tool will generate a file xref file is built with every build
GUID to guid name for that platform. This maps the GUID value to the
name.

### Where is the GUID name being used with a particular protocol?

Use the BUILD –Y to generate a report

### What GUID maps to what drivers and how to find drivers installed?

The output file Guid.xref gets generated as part of the build. This file
will list all the GUIDs and the modules that are associated for each
GUID that get built into a platform. From the shell prompt the “Drivers”
will list out the drivers installed.

### Is there a data base for all the GUIDs installed in a platform?

Yes, from the EFI Shell prompt the “dh” command will list all of the
handles with their associated GUIDs. In addition, the output file
Guid.xref gets generated as part of the build. This file will be located
in the build output directory. This file will list all the GUIDs and the
modules that are associated for each GUID

### Why is there a section GUID declared in the FDF file?

The GUIDed section is used for contiguous section guid, GUID keyword -
used for signing- compression - encryption (encapsulation section -list
of sections\] as devided in the PI Spec.

### Why use the GUID?

Because of where it gets mapped. Different compressions is another
reason. The GUID has a cross reference in the tools to call used with
tools_def.txt in the conf directory.
