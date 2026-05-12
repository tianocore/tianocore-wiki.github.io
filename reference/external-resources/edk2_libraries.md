# EDK2 Libraries

This page gives an overview of [EDK II](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II/)
libraries.

## Classes vs. Implementation

Libraries in edk2 are defined in two distinct ways. Each library has a
'class' and one or more 'implementations'.

A library class defines the C language based interface to the library.
Generally this involves prototypes of C based functions.

A library implementation is an instance of a library that implements the
library class. It is possible to have multiple implementations for the
same library class.

Code which uses a library in edk2 should generally only focus on the
library class interface. The details of which library implementation are
used can be decided at a later point, and even altered easily within the
build process.

## Classes

## Using a library class

If a driver or another library wants to make use of a library, then only
a few changes must be made.

1\. Include the package .dec file in the driver or library .inf file.
The syntax in the .inf will look like this:

    [Packages]
      MdePkg/MdePkg.dec

This will enable the code built in the driver or library to have the
package include directories added to its include path. So, when the .inf
change above is made, now the MdePkg/Include directory will be added to
the include path.

2\. Include the library class .h file. Within a .c or .h file within the
driver or library code, add the \#include. For example, to use the
MdePkg/Include/Library/CpuLib.h library class:

    #include "Library/CpuLib.h"

3\. Add the library class name to the driver or library's .inf file. For
example:

    [LibraryClasses]
      CpuLib

## Defining a class

A library class is defined in a C language .h file. A simple example is
MdePkg/Include/Library/CpuLib.h. This library class declares a C
prototype for a function:

    VOID
    EFIAPI
    CpuSleep (
      VOID
      );

In addition to defining the class .h file, you will need to ensure that
the package .dec file contains the include path necessary to include the
library class .h file. For example, in MdePkg/MdePkg.dec:

    [Includes]
      Include

Now, when a .inf file uses MdePkg/MdePkg.dec:

    [Packages]
      MdePkg/MdePkg.dec

The MdePkg/Include path will be added to the include paths for building
that module.

## Implementations

## Using a library implementation

The .dsc file used in your build process defines which library
implementation is actually used by a driver.

For example, in MdeModulePkg/MdeModulePkg.dsc, BaseMemoryLib is mapped
as:

    [LibraryClasses]
      BaseMemoryLib|MdePkg/Library/BaseMemoryLib/BaseMemoryLib.inf

But, **note** that this mapping only applies when
MdeModulePkg/MdeModulePkg.dsc is built. If another .dsc is used, it must
specify the library mappings itself, and the
MdeModulePkg/MdeModulePkg.dsc will have no impact on the mapping when
the other .dsc file is built.

## Library Constructors

If a library requires some initialization code to run, then it can
define a constructor function. For example, in
MdePkg/Library/UefiBootServicesTableLib/UefiBootServicesTableLib.inf:

      CONSTRUCTOR                    = UefiBootServicesTableLibConstructor

This function in
MdePkg/Library/UefiBootServicesTableLib/UefiBootServicesTableLib.c is
then called before the driver entry point.

## How library constructors work

During the build process, the library constructors are auto-generated to
be called from a function named ProcessLibraryConstructorList.

To see an example of this, after building look for a driver's AutoGen.c
file underneath the build directory.

The ProcessLibraryConstructorList will then be called by the entry point
library code. For an example of this, see
MdePkg/Library/UefiDriverEntryPoint/DriverEntryPoint.c.
