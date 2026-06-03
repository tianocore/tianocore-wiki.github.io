# Frequently asked EDK II build questions

New instructions: [Build Instructions](build_instructions.md)

```admonish note title="Updated Page"
This page is retained for reference. Most of the content as of 2022 is still relevant, but it is recommended to
view the new set of build instructions that describe how to develop using containers and build with the Stuart
application.
```

## Regarding the Build for EDK II, how do you specify a different compiler tool chain on the command line?

Use –t parameter for the build command. Example: Using the Microsoft
Visual Studio 2019 tool chain ...

`build –t VS2019`

For using other tools see
[Getting Started with EDK II](../../development/tutorials-howto/getting_started_with_edk_ii.md).
This provides some detailed instructions for setting up some different
tool chains? The file Conf/tools_def.txt contains a list of targets.

## Is it possible to use PCDs @ build time?

It depends on what you are trying to do. For use in code, yes. For
example Featureflag PCD type can be used. For determining if something
should be built then it might be better to use the “Build –D MACRO-NAME”
options.

## Is there information on Building on Linux?

For EDK II, yes, the build tools will need to be recompiled for GCC.
Link for how to Build for GCC:

- [Using EDK II with Native GCC](../environment-setup/using_edk_ii_with_native_gcc.md)
- [Unix-like systems](../environment-setup/unix_like_systems.md) (For older Linux
  distributions, or when using Cygwin or Mac OS X)

## What does the parsing tool do?

The parsing is part of the first stage of the build process. There are
tools for parsing the set build description files and the target.txt for
a package or platform and creates the intermediate make and autogen
files

## Regarding writing UEFI Applications in EDK II, where is the output and/or the binary UEFI application after doing a build?

The Build output directory is defined in the defines section of a .DSC
file. For example, Nt32Pkg\Nt32Pkg.dsc - the UEFI application would be
in Build\NT32\DEBUG_MYTOOLS\IA32

`OUTPUT_DIRECTORY            = Build/NT32`
`SUPPORTED_ARCHITECTURES = IA32`
`BUILD_TARGETS           = DEBUG`

## How do I get my UEFI application to the target UEFI System?

Copy the UEFI Binary image from the output directory after the build to
a USB thumb drive. Insert the USB drive in the UEFI target system. Boot
to the EFI Shell. The USB thumb drive should be one of the file systems,
e.g. FS0:. Cd to that USB drive and run your UEFI application from the
shell prompt

## Is the Build tool source code part of the Build?

No, the repository for the Tool Source is a separate project. The
binaries by default are for a Windows build machine. For building on a
non Windows machine there are instructions for recompiling the build
tools.

The sources are also in the BaseTools directory with the pre-build
Windows executables. These sources are provided because they are the
sources that were used to build the binaries. On Windows systems, the
tools do not need to be built. The pre-build binaries can be used. One
Linux, Unix, and OS/X systems, these sources are used to build the
binaries for that OS, or in the case of Python, the Python sources are
executed directly.

The [BaseTools](base_tools.md)
Source Project is where advanced development is done on the EDK II
tools. Tool developers work in this separate project until a new feature
is stable, and only once it is stable is a feature added to the
BaseTools directory and new binaries are generated.

## Can we use Ifdefs?

This is not recommended but can be used within the DSC or FDF as part of
the build. But here is an example:

`!ifdef $(SOURCE_DEBUG_ENABLE)`
`MSFT:*_*_X64_GENFW_FLAGS  = --keepexceptiontable`
`GCC:*_*_X64_GENFW_FLAGS   = --keepexceptiontable`
`INTEL:*_*_X64_GENFW_FLAGS = --keepexceptiontable`
`!endif`

## When can the report generator show the protocols produced by modules?

The report generator can show protocols produced by modules. The Runtime
DXE core will also report what is missing before handing off.

## Buffer Security Check Flag Behavior

UEFI applications and drivers are not executed in an Operating System
environment. This is important, as the switches have very specific (and,
in the pre-boot space, negative) impacts on generated code. The switch
does two things in the code that are not acceptable for the pre-boot
environment:

- The switch enables additional code in the compiled code base, which
  requires a larger stack space than may be available in the pre-boot
  environment
- The switch injects a call to a compiler specific function that is not
  present in our Firmware builds, and which we do not have information
  on how to emulate.

We disable these settings, as enabling them would create non-functioning
code.

However, please be aware that Detecting Stack Overflows in Firmware is
critical in validation and development, and we use other techniques in
our code to do so. We just cannot generically support the /Gs flags (as
noted above).

The tools do have this flag set, as they are used within the Operating
System environment, where the intrinsic added by the compiler can be
processed correctly.

## Are there Dual-mode drivers in EDK II?

No. The EDK II build system does not support the dual mode drivers
described in the PI Specification. These types of modules are very
difficult to implement correctly, so we recommend that developer
implement two different modules instead. The EDK II does allow them to
share sources, but 2 different PE/COFF images would be generated when
built.

## Is there a tool to parse the BIOS Build tree?

a\) Use the report generator build into the build tool “BUILD –Y” on the
command line

`-Y REPORTTYPE, --report-type=REPORTTYPE`
`Flags that control the type of build report to`
`generate.  Must be one of: [PCD, LIBRARY, FLASH, DEPEX,`
`BUILD_FLAGS, FIXED_ADDRESS, EXECUTION_ORDER].`
`To specify more than one flag, repeat this option on`
`the command line and the default flag set is [PCD,`
`LIBRARY, FLASH, DEPEX, BUILD_FLAGS, FIXED_ADDRESS]`
`Use “–Y DEPEX” and this will generate a text file with dependencies`

b\) Predicted dispatch order is limited because it makes assumptions
about the behavior of the modules. It cannot handle that some PPI and
DXE protocols that might be conditionally produced. Documented in the
EDK2010 March 2010 release notes.

c\) Behavior of dispatch – filter for DEBUG_DISPATCH in DSC in the PCD
for the error level PcdDebugPrintErrorLevel

## How does the build tool load the reset vector at 0xFFFFFFF0?

This is defined in the PI Specification, Volume 3.
[https://uefi.org/specs/](https://uefi.org/specs/)

In the FV (Firmware Volume) there is something called a Volume Top File
inf the FV . A Volume Top File (VTF) is a file that must be located such
that the last byte of the file is also the last byte of the firmware
volume. Regardless of the file type, a VTF must have the file name GUID
of EFI_FFS_VOLUME_TOP_FILE_GUID as defined below.

From a PI point of view the first module that runs is the SEC core. If
you look at the VTF file it is basically the code that contains the
reset vector, and it jumps to the SEC code.

Reference:
[https://github.com/tianocore/edk2/tree/master/UefiCpuPkg/ResetVector/Vtf0](https://github.com/tianocore/edk2/tree/master/UefiCpuPkg/ResetVector/Vtf0)

So the hard code bit is the FV (Firmware Volume) that that contains the
Volume Top File needs to start at an address where the end of the FV
will end up at the magic reset vector address.

In general the EFI build system constructs relocatable PE/COFF images,
and every image is linked at zero. If the code executes from FLASH, then
when the FV is constructed the PE/COFF images that are XIP (eXecute In
Place) have their PE/COFF image relocated based on where they end up in
the FV (based on info in FDF file). This is done by the build system. If
the EFI code runs from RAM then it is loaded by a PE/COFF loader and
relocated to its load address.
