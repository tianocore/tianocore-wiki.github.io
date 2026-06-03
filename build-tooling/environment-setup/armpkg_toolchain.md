# ArmPkg Toolchain

## List of ARM toolchains

| Toolchain Name | Host OS | Description |
|----|----|----|
| RVCT | Windows | Support RVCT3/4/5 versions |
| RVCTCYGWIN | Cygwin | Support RVCT3/4/5 versions |
| RVCTLINUX | Linux | Support RVCT3/4/5 versions |
| ARMGCC | Linux | Support Sourcery G++ Lite toolchain ARM EABI (tested: 2010q3) (arm-none-eabi- prefix) |
| ARMLINUXGCC | Linux | Support Linaro and ARM GNU/Linux toolchain (arm-linux-gnueabi- prefix) |
| XCODE32 | MacOS | Support XCode 3.2 |

ARM Toolchain supported in EDK2 BaseTools

**Note:** BaseTools does not need to by build when EDK2 is
built from a MS Windows. Pre-built BaseTools binaries are provided by
EDK2 repository.

## Add Support for ARMGCC

1\. Get the arm-none-eabi Toolchain from Code Sourcery:

wget
https://www.codesourcery.com/sgpp/lite/arm/portal/package7813/public/arm-none-eabi/arm-2010.09-51-arm-none-eabi-i686-pc-linux-gnu.tar.bz2
    tar xjf arm-2010.09-51-arm-none-eabi-i686-pc-linux-gnu.tar.bz2

2\. Add the arm-none-eabi toolchain to your path

## Add Support for ARMLINUXGCC

1\. On a Ubuntu based-distribution, add the Linaro toolchain:

    sudo add-apt-repository ppa:linaro-maintainers/toolchain
    sudo apt-get update
    sudo apt-get install gcc-arm-linux-gnueabi
