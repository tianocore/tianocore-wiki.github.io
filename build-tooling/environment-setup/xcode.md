# Xcode

This page provides step-by-step instructions for setting up a [EDK
II](../../reference/external-resources/edk_ii.md) build environment on macOS systems using the Xcode
development tools. These steps have been verified with macOS Big Sur 11.3.1

## macOS Xcode

Download the latest version of [Xcode](https://developer.apple.com/xcode) (12.5 as of 2021-05-09) from the Mac App
Store. After installing Xcode, you will additionally need to install the extra command-line tools. To do this, at a
Terminal prompt, enter:

```bash
xcode-select --install
```

## Additional Development Tools

While Xcode provides a full development environment as well as a suite of different utilities, it does not provide all
tools required for TianoCore development. These tools can be provided in a number of ways, but the most popular way
comes from [Brew](https://brew.sh). Installation information is provided at the previous link.

## Install mtoc

The mtoc utility is required to convert from the macOS Mach-O image format to the PE/COFF format as required by the UEFI
specification.

### Brew Instructions

```bash
brew install mtoc
```

## Install NASM

The assembler used for EDK II builds is Netwide Assembler (NASM). The latest version of NASM is available from
[https://nasm.us/](https://nasm.us/).

## Brew Instructions

```bash
brew install nasm
brew upgrade nasm
```

## Install ACPI Compiler

In order to support EDK II firmware builds, the latest version of the ASL compiler from
[https://acpica.org](https://acpica.org) must be installed. The ASL compiler is required to build ACPI Source Language
code.

## Brew Install

```bash
brew install acpica
brew upgrade acpica
```

## Install XQuartz

The EmulatorPkg requires headers from X11, which are provided by the XQuartz project. Install it from
[https://www.xquartz.org/](https://www.xquartz.org/).

## Install QEMU Emulator

On order to support running the OVMF platforms from the OvmfPkg, the QEMU emulator from
[https://www.qemu.org/](https://www.qemu.org/) must be installed.

## Brew Install

```bash
brew install qemu
brew upgrade qemu
```

## Update PATH environment variable

Tools installed using the `brew` command are placed in `/usr/local/bin`. The `PATH` environment variable must be updated
so the newly installed tools are used instead of older pre-installed tools.

```bash
export PATH=/usr/local/bin:$PATH
```

## Verify tool versions

Run the following commands to verify the versions of the tools that have been installed.

```bash
nasm –v
iasl –v
qemu-system-x86_64 --version
mtoc
```

## Checkout edk2 From Source Control

Pick the location you want to down load the files to and `cd` to that directory:

```bash
cd ~/work
git clone https://github.com/tianocore/edk2.git
cd edk2
git submodule update --init
```

## Build from Command Line/Debug with lldb

Build the EmulatorPkg:

```bash
cd ~/work/edk2/EmulatorPkg
./build.sh
```

Debug the EmulatorPkg

```bash
./build.sh run

Initializing workspace
/Users/bcran/src/edk2/BaseTools
Loading previous configuration from /Users/bcran/src/edk2/Conf/BuildEnv.sh
Using EDK2 in-source Basetools
WORKSPACE: /Users/bcran/src/edk2
EDK_TOOLS_PATH: /Users/bcran/src/edk2/BaseTools
CONF_PATH: /Users/bcran/src/edk2/Conf
using prebuilt tools
(lldb) target create "./Host"
Current executable set to '/Users/bcran/src/edk2/Build/EmulatorX64/DEBUG_XCODE5/X64/Host' (x86_64).
(lldb) command script import /Users/bcran/src/edk2/EmulatorPkg/Unix/lldbefi.py
Type r to run emulator. SecLldbScriptBreak armed. EFI modules should now get source level debugging in the emulator.
(lldb) script lldb.debugger.SetAsync(True)
(lldb) run
Process 12155 launched: '/Users/bcran/src/edk2/Build/EmulatorX64/DEBUG_XCODE5/X64/Host' (x86_64)

EDK II UNIX Host Emulation Environment from https://github.com/tianocore/tianocore.github.io/wiki/EDK-II/
  BootMode 0x00
  OS Emulator passing in 128 KB of temp RAM at 0x102000000 to SEC
  FD loaded from ../FV/FV_RECOVERY.fd at 0x102020000 contains SEC Core
...
```

Type `process interrupt` at the lldb prompt (don't forget to hit carriage return) to pause execution. Ctrl-c in the
terminal window will quit lldb. `bt` is the stack backtrace command:

```bash
Process 12420 stopped
* thread #1, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
    frame #0: 0x00007fff2033cc22 libsystem_kernel.dylib:__semwait_signal() + 10
libsystem_kernel.dylib`__semwait_signal:
->  0x7fff2033cc22 <+10>: jae    0x7fff2033cc2c            ; <+20>
    0x7fff2033cc24 <+12>: movq   %rax, %rdi
    0x7fff2033cc27 <+15>: jmp    0x7fff2033b72d            ; cerror
    0x7fff2033cc2c <+20>: retq
Target 0: (Host) stopped.
(lldb) bt
* thread #1, queue = 'com.apple.main-thread', stop reason = signal SIGSTOP
  * frame #0: 0x00007fff2033cc22 libsystem_kernel.dylib:__semwait_signal() + 10
    frame #1: 0x00007fff202bcc2a libsystem_c.dylib:nanosleep() + 196
    frame #2: 0x0000000100005e55 Host:SecCpuSleep() + 37 at /Users/bcran/src/edk2/EmulatorPkg/Unix/Host/EmuThunk.c:334
    frame #3: 0x000000010000e96e Host:GasketSecCpuSleep() + 11 at /Users/bcran/src/edk2/Build/EmulatorX64/DEBUG_XCODE5/X64/EmulatorPkg/Unix/Host/Host/OUTPUT/X64/Gasket.iiii:283
    frame #4: 0x0000000106f985e9 DxeCore.dll:CoreDispatchEventNotifies() + 264 at /Users/bcran/src/edk2/MdeModulePkg/Core/Dxe/Event/Event.c:194
    frame #5: 0x0000000106f97fce DxeCore.dll:CoreRestoreTpl() + 227 at /Users/bcran/src/edk2/MdeModulePkg/Core/Dxe/Event/Tpl.c:131
    frame #6: 0x0000000106f989db DxeCore.dll:CoreSignalEvent() + 111 at /Users/bcran/src/edk2/MdeModulePkg/Core/Dxe/Event/Event.c:566
    frame #7: 0x0000000106f98b01 DxeCore.dll:CoreWaitForEvent() + 94 at /Users/bcran/src/edk2/MdeModulePkg/Core/Dxe/Event/Event.c:707
    frame #8: 0x0000000113e8e54c BdsDxe.dll:BdsWaitForSingleEvent() + 127 at /Users/bcran/src/edk2/MdeModulePkg/Universal/BdsDxe/BdsEntry.c:250
    frame #9: 0x0000000113e8e70b BdsDxe.dll:BdsWait() + 215 at /Users/bcran/src/edk2/MdeModulePkg/Universal/BdsDxe/BdsEntry.c:328
    frame #10: 0x0000000113e8dffb BdsDxe.dll:BdsEntry() + 2612 at /Users/bcran/src/edk2/MdeModulePkg/Universal/BdsDxe/BdsEntry.c:1012
    frame #11: 0x0000000106f9bbd6 DxeCore.dll:DxeMain() + 2791 at /Users/bcran/src/edk2/MdeModulePkg/Core/Dxe/DxeMain/DxeMain.c:551
    frame #12: 0x0000000106f9ed8f DxeCore.dll:_ModuleEntryPoint() + 20 at /Users/bcran/src/edk2/MdePkg/Library/DxeCoreEntryPoint/DxeCoreEntryPoint.c:48
    frame #13: 0x0000000106fdd02f DxeIpl.dll:InternalSwitchStack() + 15
    frame #14: 0x0000000106fdc0b6 DxeIpl.dll:HandOffToDxeCore() + 546 at /Users/bcran/src/edk2/MdeModulePkg/Core/DxeIplPeim/X64/DxeLoadFunc.c:126
    frame #15: 0x0000000106fda78a DxeIpl.dll:DxeLoadCore() + 1354 at /Users/bcran/src/edk2/MdeModulePkg/Core/DxeIplPeim/DxeLoad.c:449
    frame #16: 0x0000000106ff1d7c
    frame #17: 0x00000001020255c6 PeiCore.dll:PeiCore() + 1982 at /Users/bcran/src/edk2/MdeModulePkg/Core/Pei/PeiMain/PeiMain.c:331
    frame #18: 0x000000010202a82c PeiCore.dll:PeiCheckAndSwitchStack() + 1171 at /Users/bcran/src/edk2/MdeModulePkg/Core/Pei/Dispatcher/Dispatcher.c:842
    frame #19: 0x000000010202b853 PeiCore.dll:PeiDispatcher() + 1206 at /Users/bcran/src/edk2/MdeModulePkg/Core/Pei/Dispatcher/Dispatcher.c:1609
(lldb)

```

## Build and Debug from Xcode

To build from the Xcode GUI open ~/work/edk2/EmulatorPkg/Unix/Xcode/xcode_project64/xcode_project.xcodeproj. You can
build, clean, and source level debug from the Xcode GUI. You can hit the Build and Debug button to start the build
process. You need to need to hit command-shift-B to show the output of the build. Click Pause to break into the
debugger.

The stack trace contains items that show as ?? since the default shell is checked in as a binary. `nanosleep$UNIX2003`
and `__semwait_signal` are POSIX library calls and you do not get C source debug with these symbols.

*Note* The Xcode project is currently (as of 2021-05-09) broken.

## See Also

## Continue with common instructions

The [remaining instructions](../build-workflows/common_instructions_for_unix.md) are
common for most UNIX-like systems.
