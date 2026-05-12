# Tasks Text Editor

This task involves making several improvements for the standard shell
text editor

- Status: Complete :heavy_check_mark:
- Difficulty: Medium
- Language: C
- Mentor: [https://github.com/jljusten](https://github.com/jljusten)
- Suggested by: [https://github.com/jljusten](https://github.com/jljusten),
  [https://github.com/ErikBjorge](https://github.com/ErikBjorge),
  [https://github.com/jcarsey](https://github.com/jcarsey)

## Status

- Completed :heavy_check_mark:
- Work done by Suyu Yang for
  [GSOC2011](../../community/events-outreach/gsoc2011.md).
  - Source: [https://code.google.com/p/edk2-editor](https://code.google.com/p/edk2-editor)
- Available in main edk2 tree

Issues:

- Plenty of wasted screen space
- Unusual hot-keys
- Hot-keys are not usable over a serial/network terminal connection

Nice to have:

- Support terminal widths as narrow as 60 columns

Location of editor code:
[ShellPkg/Library/UefiShellDebug1CommandsLib/Edit](https://github.com/tianocore/edk2/tree/master/ShellPkg/Library/UefiShellDebug1CommandsLib/Edit)

## Development environment

Building

- This project can be completed on any edk2 supported OS or toolchain.

Testing

- This project can be tested on any PI 1.2 system.
- [OVMF](https://www.tianocore.org/ovmf/),
  [Nt32Pkg](../../archives/platforms-packages/nt32_pkg.md) or
  [UnixPkg](../../archives/platforms-packages/unix_pkg.md) could
  each provide friendly test environments.

## Current State

Here is the current look of the UEFI Editor:

    UEFI EDIT 2.0     NewFile.txt




















       Row: 1  Col: 1       0 Lines Read                            |INS|
     F1  Go To Line      F2  Save File       F3  Exit
     F4  Search          F5  Search/Replace  F6  Cut Line
     F7  Paste Line      F8  Open File       F9  File Type

## Suggested New Layout

## Main window

    UEFI EDIT 2.0     NewFile.txt























     1,1                                            Help: Ctrl-?

## Editor Help

Ctrl-? should popup a help box, something like this:

    UEFI EDIT 2.0     NewFile.txt

      +-- Help --------------------------------------------+
      | Ctrl-G, F1  Go To Line      Ctrl-S, F2  Save File  |
      | Ctrl-Q, F3  Exit            Ctrl-F, F4  Search     |
      | Ctrl-R, F5  Search/Replace  Ctrl-K, F6  Cut Line   |
      | Ctrl-U, F7  Paste Line      Ctrl-O, F8  Open File  |
      | Ctrl-T, F9  File Type                              |
      +----------------------------------------------------+















     1,1                                            Help: Ctrl-?

Another option might be to display help as if it were a text file:

    UEFI EDIT 2.0     (Editor Help)
    Help

     Control Key   Function Key   Command
     -----------   ------------   -----------------
     Ctrl-G        F1             Go To Line
     Ctrl-S        F2             Save File
     Ctrl-Q        F3             Exit
     Ctrl-F        F4             Search
     Ctrl-R        F5             Search/Replace
     Ctrl-K        F6             Cut Line
     Ctrl-U        F7             Paste Line
     Ctrl-O        F8             Open File
     Ctrl-T        F9             File Type
     Ctrl-W                       Close File

     Use Ctrl-W to exit this help







     1,1                                            Help: Ctrl-?

## Search

Ctrl-F should ask for text to find on the bottom line:

    UEFI EDIT 2.0     NewFile.txt























     Search for: UserEntersSearchTextHere

## Search/Replace

Ctrl-R should ask for the seach/replace on the bottom line. First the
search, then the replace text.

    UEFI EDIT 2.0     NewFile.txt























     Replace with: UserEntersReplacementTextHere

## See Also

- [Tasks](tasks.md)
- [GSOC2011#Shell editor
  improvements](../../community/events-outreach/gsoc2011.md#shell-editor-improvements)
- [SimpleTextIn.h](https://github.com/tianocore/edk2/blob/master/MdePkg/Include/Protocol/SimpleTextIn.h),
  [SimpleTextInEx.h](https://github.com/tianocore/edk2/blob/master/MdePkg/Include/Protocol/SimpleTextInEx.h)
