# Tasks Build A C Compiler For EBC

Build a C compiler that is able to generate EBC (EFI Byte Code)

**:red_circle: This project is not recommended as the relevance of EBC has waned. :red_circle:**

* Difficulty: Hard
* Language: C, Assembly
* Mentor:
* Suggested by: [@nate-desimone](https://github.com/nate-desimone)

## Background

**EBC is no longer required by the UEFI spec, which has caused its use for cross-architecture OpROMs to asymptotically
approach zero.**

EBC (EFI Byte Code) is an interpreted intermediate language for building processor independent UEFI device drivers.
Unlike most other intermediate languages, EBC was primarily designed for compiling portable C code in an architecture
independent way. The reason it was invented is to enable PCIe add-in card vendors to build universal Option ROMs
(OpROMs) that function on multiple native instruction set architectures. For example, a PCIe add-in card with a EBC
OpROM would be able to work out-of-box on both a regular 64-bit x86 PC and a ARM microserver.

Historically, there are some old proposals for EBC usage that never materialized. It was suggested that an EBC
interpreter could be added to operating systems to enable EBC device drivers to be used in the OS as a "plan B" for when
OS native drivers are not available. The two use cases considered were graphics and network. EBC graphics drivers would
be used for basic display output immediately after the OS was installed, but before the user has been able to install OS
native graphics drivers. The currently used UEFI GOP drivers provide this capability as well, but EBC graphics drivers
would have enabled resolution changes at runtime, similar to legacy VGA BIOS. This feature that was lost when the
industry moved to UEFI GOP drivers. UEFI UNDIs could be used as a basic network drivers for cases where the OS media did
not have the needed network drivers in-box. The UNDI would allow the system to access the Internet to download OS native
drivers. Vincent Zimmer has a good history of EBC on his [blog](https://vzimmer.blogspot.com/2015/08/efi-byte-code.html).

EBC has a unique feature that is not found in other intermediate languages, natural indexing. Natural indexing has made
compiler development for EBC notoriously difficult. In EBC, there are several basic types that are variable in size. For
example, the `sizeof(void *)` as well as any other pointer type can vary at runtime. On 32-bit host systems like IA32,
`sizeof(void *)==4`. On 64-bit systems like X64 or AARCH64, `sizeof(void *)==8`. The UEFI specific integer data type,
`UINTN` uses the same variable sizes as pointers in EBC.

## Existing Work

The original EBC compiler is the Intel C Compiler for EFI Byte Code. This compiler is very expensive ($955) which has
likely been the largest component in EBC's limited popularity. The usefulness of this compiler has waned since the
development of EDK II. This compiler does not support LTO (Link Time Optimization). The LibraryClass dependency system
in EDK II relies heavily on LTO in order to generate reasonably sized binaries.

[Akira Moroo](https://retrage.github.io/about/) ([@retrage](https://github.com/retrage)) has done a great deal of work
on developing an open source EBC compiler. He has made 2 attempts thus far and has talked about his work both on his
[blog](https://translate.google.com/translate?hl=&sl=ja&tl=en&u=https%3A%2F%2Fretrage.github.io%2F2019%2F07%2F20%2Fllvm-backend-for-ebc.html)
and at [Kernel/VM Tokyo 2019](https://speakerdeck.com/retrage/llvm-backend-development-for-efi-byte-code). His [first
attempt](https://github.com/retrage/elvm/tree/retrage/ebc-v2) used ELVM but ultimately proved to be too limited due to
ELVM's lack of a linker, requiring the entire program to come from a single .c file.

His [second attempt](https://github.com/yabits/llvm/tree/retrage/ebc) uses Clang & LLVM. While this showed great
promise, it has not been completed yet. One of the roadblocks at the time was the lack of a PE/COFF linker for LLVM.
Since LLVM 9.0, the LLVM linker now supports PE/COFF output, making a version of clang that can output EBC more
attainable. Rebasing his work up to the latest LLVM 11 is the recommended starting point for any future work.

## Development Environment

Building: This project should ideally support all edk2 supported OSes when it is finished. Though Linux would likely be
the easiest starting point since the existing LLVM work was done on Linux. Getting the compiler stable on Linux should
be the first goal. Testing generated EBC binaries can be done with
[EmulatorPkg](https://github.com/tianocore/edk2/tree/master/EmulatorPkg) without the need for any emulation software
like Qemu. [@retrage](https://github.com/retrage) has also created his own userspace
[ebcvm](https://github.com/yabits/ebcvm), however we would recommend EmulatorPkg over it since EmulatorPkg runs the
exact EBC interpreter that will be used on real systems.

## Links for More Information

* [https://github.com/yabits/llvm/tree/retrage/ebc](https://github.com/yabits/llvm/tree/retrage/ebc)
  [https://translate.google.com/translate?hl=&sl=ja&tl=en&u=https%3A%2F%2Fretrage.github.io%2F2019%2F07%2F20%2Fllvm-backend-for-ebc.html](https://translate.google.com/translate?hl=&sl=ja&tl=en&u=https%3A%2F%2Fretrage.github.io%2F2019%2F07%2F20%2Fllvm-backend-for-ebc.html)
  [https://speakerdeck.com/retrage/llvm-backend-development-for-efi-byte-code](https://speakerdeck.com/retrage/llvm-backend-development-for-efi-byte-code)
* [https://vzimmer.blogspot.com/2015/08/efi-byte-code.html](https://vzimmer.blogspot.com/2015/08/efi-byte-code.html)
* [https://github.com/tianocore/edk2/tree/master/EmulatorPkg](https://github.com/tianocore/edk2/tree/master/EmulatorPkg)
* [https://github.com/yabits/ebcvm](https://github.com/yabits/ebcvm)
* [https://github.com/pbatard/fasmg-ebc](https://github.com/pbatard/fasmg-ebc)
* [https://github.com/retrage/elvm/tree/retrage/ebc-v2](https://github.com/retrage/elvm/tree/retrage/ebc-v2)

## Further Discussion

Interested parties are welcome to discuss this project on [edk2-devel](https://edk2.groups.io/g/devel).

## See Also

[Tasks](tasks.md)
