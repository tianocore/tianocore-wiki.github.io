# Shell FAQ

[shell](../../platforms-packages/component-guides/shell.md)

Shell questions

### Can you write a shell app in EBC?

Yes, it is possible. Please check the ULA for the EBC compiler.
[1](https://software.intel.com/sites/products/documentation/EULA/Intel_SW_Dev_Products_EULA.pdf)
I believe that has the specific details on driver/app support. There are
no known technical issues with using EBC for Shell Applications or UEFI
Applications. This is mainly a validation issue. Another issue is that
the UEFI 2.3 Specification Section 2.6.2 item (19), states that the EBC
interpreter is only required if a platform intends to support EBC
drivers in containers for add in devices. This means that the EBC
interpreter is optional and may not be available on all platforms. As a
result, a Shell Application or UEFI Application that is compiled for EBC
may not be able to run on all platforms. Intel® C Compiler for EFI Byte
Code Product In-Depth

### Can we do shell within shell & script within script?

Yes, the shell can invoke nested instances of the shell and shell
scripts can also invoke nested instances of other scripts

### Is the location of scripts fixed?

No, Shell scripts can exist anywhere within a Fat file system on any
media. If they are not in the CWD then the path to a script can be
specified.

### Can you add driver into your runtime from an external USB or other drive?

Yes, can boot to shell and load with shell’s load command—can be a BDS
option or others.

### Is there an event related to the efi shell?

No, but it is in Preboot time and after Architecture protocols.

### Are there any signing tools for using in the EFI shell to sign a capsule of Firmware volume?

We provide a way from the enabling perspective by using a GUIDed section
extraction and EDK II has tools to recognize a GUIDed section. This
would enable Compression, Encryption and Signing. There are sources for
tools in BaseTools CRC32 and LZMA compression. By looking a these
examples a signing tool can also be used.

### When is signing done and what about PEI and DXE signature checks

It is done at Build time, but it is possible to sign at boot time. For
PEI and DXE signature checks it is possible as long as they are build
with the appropriate architecture (E.g. 32 bit fore PEI and X64 for DXE)

### What tool collects all the drivers and what order does the driver dispatcher execute the drivers?

The mechanism is in the PI Spec. The concept is that for every driver
there is a dependency expression - UEFI Drivers implied dependence on
architecture protocols and DXE Services. The PEI and DXE dispatcher is
responsible for checking each driver's dependency and determines whether
to dispatch/execute each driver as it finds it in the flash device. If
the driver's dependency expression does not evaluate to TRUE it will set
that driver aside and continue to the next driver in the flash device.
It is possible some drivers may not get dispatched.

### Does it mean that Dispatched is accessing the flash searching for the dependency expressions?

The code will access the flash as little as possible because of the
performance. For example, in PEI there is use of Cache memory if a
driver's dependency is not TRUE.

### How does PEI core allocate Cache for keeping track of PEIMs that do not get dispatched?

No eviction mode and set a side temp memory. PEI core will allocate
memory.

There is another method called an a priori file. There can be one per
Firmware volume. This will tell the dispatcher what order to dispatch
list drivers in the order they need to dispatch them. This is a manual
order. It does not need to be in any specific place in the FV. If it in
the FV the dispatcher will find it.

### How do you add the new UEFI Shell 2.0 to the Nt32 in the UDK2010.SR1

You need to update the Nt32Pkg.dsc and the Nt32Pkg.fdf with the
following:

**Nt32Pkg.dsc**

1\) At the end in the \[LibraryClasses.common.UEFI_APPLICATION\] section
add the following:

`ShellLib|ShellPkg/Library/UefiShellLib/UefiShellLib.inf`
`FileHandleLib|ShellPkg/Library/UefiFileHandleLib/UefiFileHandleLib.inf`
`SortLib|ShellPkg/Library/UefiSortLib/UefiSortLib.inf`
`ShellCEntryLib|ShellPkg/Library/UefiShellCEntryLib/UefiShellCEntryLib.inf`
`PathLib|ShellPkg/Library/BasePathLib/BasePathLib.inf`

2\) At the end in the \[PcdsFixedAtBuild\] Section add the following:

`#   Update the PCD as follows using the UEFI Shell's 2.0 PCD:`
`gEfiIntelFrameworkModulePkgTokenSpaceGuid.PcdShellFile|{ 0x83, 0xA5, 0x04, 0x7C, 0x3E, 0x9E, 0x1C, 0x4F, 0xAD, 0x65, 0xE0, 0x52, 0x68, 0xD0, 0xB4, 0xD1 }`

3\) At the end in the \[Components.IA32\] Section add the following:

`ShellPkg/Application/Shell/Shell.inf {`
``
`NULL|ShellPkg/Library/UefiShellLevel2CommandsLib/UefiShellLevel2CommandsLib.inf`
`NULL|ShellPkg/Library/UefiShellLevel1CommandsLib/UefiShellLevel1CommandsLib.inf`
`NULL|ShellPkg/Library/UefiShellLevel3CommandsLib/UefiShellLevel3CommandsLib.inf`
`FileHandleLib|ShellPkg/Library/UefiFileHandleLib/UefiFileHandleLib.inf`
`ShellCommandLib|ShellPkg/Library/UefiShellCommandLib/UefiShellCommandLib.inf`
`SortLib|ShellPkg/Library/UefiSortLib/UefiSortLib.inf`
`HandleParsingLib|ShellPkg/Library/UefiHandleParsingLib/UefiHandleParsingLib.inf`
`ShellLib|ShellPkg/Library/UefiShellLib/UefiShellLib.inf`
`ShellCEntryLib|ShellPkg/Library/UefiShellCEntryLib/UefiShellCEntryLib.inf`
`PathLib|ShellPkg/Library/BasePathLib/BasePathLib.inf`
`!ifndef $(NO_SHELL_PROFILES)`
`NULL|ShellPkg/Library/UefiShellDriver1CommandsLib/UefiShellDriver1CommandsLib.inf`
`NULL|ShellPkg/Library/UefiShellInstall1CommandsLib/UefiShellInstall1CommandsLib.inf`
`NULL|ShellPkg/Library/UefiShellDebug1CommandsLib/UefiShellDebug1CommandsLib.inf`
`NULL|ShellPkg/Library/UefiShellNetwork1CommandsLib/UefiShellNetwork1CommandsLib.inf`
`!endif`
``
`gEfiShellPkgTokenSpaceGuid.PcdShellLibAutoInitialize|FALSE`
`}`

**Nt32Pkg.fdf**

4\) Add the following at the end of the \# DXE Phase modules in the
Nt32Pkg.fdf file

`INF  ShellPkg/Application/Shell/Shell.inf`

5\) Comment out by adding "#" at the begining of the line the EFI EDK
shell 1.0 in the FILE section of the Nt32Pkg.fdf file

`#FILE APPLICATION = PCD(gEfiIntelFrameworkModulePkgTokenSpaceGuid.PcdShellFile) {`
`#    SECTION PE32 = EdkShellBinPkg/FullShell/Ia32/Shell_Full.efi`
`#  }`
