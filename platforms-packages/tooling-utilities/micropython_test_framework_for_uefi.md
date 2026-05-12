# MicroPython Test Framework for UEFI

[https://github.com/tianocore/edk2-staging/tree/MicroPythonTestFramework](https://github.com/tianocore/edk2-staging/tree/MicroPythonTestFramework)

The MicroPython Test Framework for UEFI (`MpyTestFrameworkPkg`) is designed for firmware unit testing and validation.
This framework provides a set of convenient abstractions designed to remove as much boilerplate as possible from
firmware test configuration, case development, and test execution. It is general enough to be useful in a variety of
firmware testing scenarios including black box tests, white box tests, functional testing, and automating UI/human
interaction.

[https://github.com/tianocore/edk2-staging/tree/MicroPythonTestFramework/MpyTestFrameworkPkg](https://github.com/tianocore/edk2-staging/tree/MicroPythonTestFramework/MpyTestFrameworkPkg)

The framework leverages [MicroPython](https://micropython.org) for a lightweight and minimalist implementation. A port
of the [MicroPython](../../development/tutorials-howto/micropython.md) Interpreter for UEFI is available in
edk2-staging: (`MicroPythonPkg`).

[https://github.com/tianocore/edk2-staging/tree/MicroPythonTestFramework/MicroPythonPkg](https://github.com/tianocore/edk2-staging/tree/MicroPythonTestFramework/MicroPythonPkg)

This project was publicly announced in March 2018 and added to the
[edk2-staging](https://github.com/tianocore/edk2-staging) branch in August 2018.

## Code Structure

Commits are based on latest UDK2018, and include three packages:

* MicroPythonPkg: MicroPython Interpreter for UEFI
* MpyTestFrameworkPkg: MicroPython Test Framework for UEFI
* MpyTestCasePkg: One placeholder to contain the future platform test cases repo.

See README.md files in Package folders for more information.

The following git commands sync with the latest branch:

```
git clone https://github.com/tianocore/edk2-staging
cd edk2-staging
git checkout MicroPythonTestFramework
git submodule update --init –recursive
```

_Note: The last command will sync all submodule sources_

## References

MicroPython Project Website: [https://micropython.org/](https://micropython.org/)

Implementing MicroPython as a UEFI Test Framework

* [Intel Evangelists
  Blog](https://software.intel.com/en-us/blogs/2018/03/08/implementing-micropython-as-a-uefi-test-framework)
* [Spring 2018 UEFI Plugfest
  Presentation](https://www.uefi.org/sites/default/files/resources/Intel_Implementing%20MicroPython%20as%20a%20UEFI%20Test%20Framework%20.pdf)
