# ShellPkg (UEFI Shell 2.x)

**[ShellPkg](https://github.com/tianocore/edk2/tree/master/ShellPkg)**
is an
[EDK II
Package](edkii_packages.md) that provides a native implementation of the UEFI Shell 2.x
specifications.

[1](https://github.com/tianocore/edk2/tree/master/ShellPkg)

## Getting ShellPkg

This package provides the shell application, a set of NULL-named
libraries that provide configurable command sets, and libraries for
creating additional applications and UEFI Shell commands.

## ShellPkg versus ShellBinPkg

**ShellPkg** provides source code for the UEFI Shell and related
applications. **ShellBinPkg** provides a pre-built version of the UEFI
Shell and applications. Refer to the [ShellBinPkg
ReadMe](https://github.com/tianocore/edk2/blob/master/ShellBinPkg/ReadMe.txt)
for more info.

## UEFI Shell 2.x Engineering Resources

- [Shell Execution Requirements](../component-guides/shell.md)
- [Shell Library Primer](../component-guides/shell.md)
- [Creating a Shell Application](../../development/tutorials-howto/creating_a_shell_application.md)
- [Porting an EDK Shell
  Extension](../../development/tutorials-howto/porting_an_edk_shell_extension.md)
- [Move a Shell
  Application to internal command](../component-guides/shell_application_to_internal_command.md)
- [Shell FAQ](../../reference/faqs-glossaries/shell_faq.md)

## UEFI Shell Specifications (uefi.org)

[2](https://www.uefi.org/sites/default/files/resources/UEFI_Shell_2_2.pdf)

[3](https://www.uefi.org/sites/default/files/resources/UEFI_Shell_Spec_2_1_July02release.pdf)

[4](https://www.uefi.org/sites/default/files/resources/UEFI_Shell_Spec_2_0_Errata_A.pdf)

[5](https://www.uefi.org/sites/default/files/resources/UEFI_Shell_Spec_2_0.pdf)

## ShellPkg versus EdkShellPkg

The **[EdkShellPkg](../../archives/platforms-packages/edk_shell_pkg.md)** implements an older version of the
EFI Shell, prior to standardization by the UEFI Forum. Please treat
*EdkShellPkg**and**EdkShellBinPkg*' as end-of-life code.

[6](https://github.com/tianocore/edk2/tree/master/EdkShellPkg)

[7](https://github.com/tianocore/edk2/tree/master/EdkShellBinPkg)
