# GitHub Help

`Note: New build instructions are available. It is recommended to start with the new instructions if learning how to`
`build edk2 for the first time. This page is retained for reference.`

New instructions: [Build Instructions](../build-workflows/build_instructions.md)

GitHub ([https://help.github.com/index.html](https://help.github.com/index.html)) provides step-by-step
instructions for user registration and basic features supported by
GitHub

- Setup GitHub for Linux \| Windows \| MAC \| All
  ([https://help.github.com/articles/set-up-git](https://help.github.com/articles/set-up-git))
- To download and install a Git GUI interface ([https://git-scm.com/](https://git-scm.com/))

## GitHub EDK II Project Repositories

- The EDK II BaseTools are part of the EDK II project repository.
- The EDK II project repository is available at
  [https://github.com/tianocore/edk2](https://github.com/tianocore/edk2).
- Prebuilt Windows tools are available at
  [https://github.com/tianocore/edk2-BaseTools-win32](https://github.com/tianocore/edk2-BaseTools-win32).
- Content that is not released under an accepted open source license can
  be found at [https://github.com/tianocore/edk2-non-osi](https://github.com/tianocore/edk2-non-osi).

## Getting the extra tools

## Compiler

For both the BuildTools and [EDK II](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II/)
projects, you will need to obtain a compiler from somewhere else. These
instructions do not cover obtaining or installation of a compiler tools
chain. The BaseTools build requires a C compiler; an assembler or ACPI
assembler are not required to build tools in this project. BaseTools
assume that a compiler is already configured in the environment.

## Python

The BaseTools build supports the Makefile based build for EDK II. All
Tools are written in either C or Python. The C tools must compile on all
operating systems with various compilers, so the code is written using
simple, standard functions and libraries. The Python-based tools are
written assuming the features that were available in Python. GUI
applications can be created in Python, using the wxPython package and
all Python applications can be converted to run under a native OS
(Windows, Linux or OS/X.) via the
[Python
Tools](../../platforms-packages/tooling-utilities/python_tools.md).

The tools in this section are NOT required to build the EDK II project;
they are needed to compile the BaseTools used to build the EDK II
project.

- Linux and OS/X developers using a command line, create a directory
  that will be used to hold the files, then change to that directory.
  - Replace URL with apropriate one from
    [Python
    Tools](../../platforms-packages/tooling-utilities/python_tools.md). Installation and configuration for these tools are left
    to the developer.

Refer to documentation in either the UserManuals or Source folders for
more information.

- BuildTools [User
  Manuals](https://github.com/tianocore/buildtools-BaseTools/tree/master/UserManuals)

## How to Setup the EDK II Tree

See [Getting Started with EDK II](../../development/tutorials-howto/getting_started_with_edk_ii.md)

## EDK II Development Process

After setting up your build environment see
[EDK II Development Process](../../development/contribution-guides/edk_ii_development_process.md) for
making contributions to the EDK II Project.

## Further Help

If you have questions about the code or run into obstacles getting
things to work for you, please join our
[edk2-devel](../../community/communications/mailing_lists.md)
email list and ask your EDK II related questions on the list.
