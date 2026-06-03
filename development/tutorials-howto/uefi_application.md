# UEFI application

UEFI application questions

### Are there any guidelines for developing UEFI code in Runtime?

Currently there are only guidelines developing UEFI code for pre-boot.
Link to the EDK II module writers guide: [EDK_II Module Writer_s
Guide_0_7.pdf](https://github.com/tianocore-docs/Docs/blob/main/User_Docs/EDK_II%20Module%20Writer_s%20Guide_0_7.pdf)

To develop UEFI code in runtime the applications the UEFI Specification
defines the rules for writing runtime safe code along with the calling
conventions and the rules that must be followed for UEFI runtime code to
work correctly when the OS is running.

### What would a UEFI application be allowed to do at runtime?

UEFI Applications may only execute in the pre-boot environment prior to
ExitBootServices(). This means it is not possible for UEFI applications
to be available at OS runtime. The only exception is a special class of
UEFI application that is a UEFI OS Loader.

### How do I view the sample program “HelloWorld”?

The “HelloWorld” is a sample application that is part of the
*MdeModulePkg* in the edk2 directory [https://github.com/tianocore/edk2](https://github.com/tianocore/edk2)
repository directory. There is both a
[HelloWorld.C](https://github.com/tianocore/edk2/tree/master/MdeModulePkg/Application/HelloWorld/HelloWorld.c)
for the source and an
[HelloWorld.INF](https://github.com/tianocore/edk2/tree/master/MdeModulePkg/Application/HelloWorld/HelloWorld.inf)
file to build the module under EDK II.

A simple text editor can be used to view both of these files and make
changes.

### Is there a way to have applications start automatically?

If you are running from the shell then there are 2 ways.

1. The [shell](../../platforms-packages/component-guides/shell.md) will
    search for a script file called “startup.nsh” in any FAT file system
    that would get executed. Inside the script other UEFI applications
    could be invoked.
2. The shell's command line can specify a UEFI application to run
    automatically.

Outside the shell there are different ways.

1. The UEFI Specification also defines Boot Options. BootOption and
    BootOrder variables can be set to a UEFI Application can be auto
    executed on every boot.

The Boot Maintenance Manager provides menus to manually add a new Boot
Option and re-order the active Boot Options. A UEFI Application can also
be used to update the EFI Variables associated with Boot Options to add
a new Boot Option to a platform. This would be the typical method used
by an installer application. UEFI OS installers also use this method of
updating the EFI Variables to add a newly installed operating system to
the set of operating systems installed on a platform.
