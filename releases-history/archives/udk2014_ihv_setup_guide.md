# Udk2014.Ihv Setup Guide

## UDK2014 IHV Driver Developer Release (UDK2014.IHV)

### Please note

It is recommended to use a more up to date version of EDK II. Please see
the [UDK](udk.md) wiki page for the
latest release.

---

Reduced distribution of the
[UDK2014](udk2014.md)
specifically for independent hardware vendors (IHV) creating UEFI
drivers.

- EDK II
  [BaseTools](../../build-tooling/build-workflows/base_tools.md)
  for Windows and Linux
- [MdeModulePkg](../../platforms-packages/core-packages/mde_module_pkg.md)
- [MdePkg](../../platforms-packages/core-packages/mde_pkg.md)
- [Release
  Notes](https://sourceforge.net/projects/edk2/files/UDK2014_Releases/UDK2014/UDK2014.IHV-ReleaseNotes.txt/download)
  and Documentation

This subset of packages is designed to be used with the
[UEFI Driver Wizard](../../platforms-packages/tooling-utilities/uefi_driver_wizard.md).

## Setup and Configuration

1. Download the UDK2014 IHV package
    ([UDK2014.IHV](https://github.com/tianocore-docs/Docs/blob/main/Driver_Developer/UDK2014.IHV.zip)).
2. Unzip
    [UDK2014.IHV](https://github.com/tianocore-docs/Docs/blob/main/Driver_Developer/UDK2014.IHV.zip)
    into a workspace directory (example: C:\FW\UDK2014.IHV)
3. Unzip the BaseTools ZIP file into the workspace directory
4. Open a Visual Studio Command Prompt and run edksetup.bat in the
    workspace directory
