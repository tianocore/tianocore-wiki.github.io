# Code Style C

This is just a brief overview of the C
[Code Style](code_style.md)
used for our projects. The EDK II Project requires the use of
[https://github.com/uncrustify/uncrustify](https://github.com/uncrustify/uncrustify)
to automatically format C code to match the EDK II C Coding Standard.
Details on how to install and run uncrustify for EDK II can be found at
[EDK II Code Formatting](edk_ii_code_formatting.md).

The official C Coding Standards specification can be found at:

- EDK II C Coding Standards 2.4
  [HTML](https://tianocore-docs.github.io/edk2-CCodingStandardsSpecification/release-2.40/)
  \|
  [.PDF](https://tianocore-docs.github.io/edk2-CCodingStandardsSpecification/release-2.40/edk2-CCodingStandardsSpecification-release-2.40.pdf)

## Overview

- CamelCase used for variables, functions and file names
- UPPERCASE used for types and macros
- Use UEFI types rather than C types
  - int=\>INTN; unsigned int=\>UINTN; void=\>VOID; etc...
- Limit line length to 80 characters
- 2 spaces of indentation
- Never use tab characters.
  - Set editor to insert spaces rather than a tab character.
- if, for, while, etc. always use { }, even when there is only one
  statement
  - The opening brace ({) should always appear at the end of the line
    previous line.
- The opening brace ({) for a function should always appear separately
  on the a new line.

## Example of Properly Formatted Code

C File Example:

`#include "FooFileName.h"`

`/**`
`Brief and Detailed Descriptions.`

`@param[in]      Arg1 Description of Arg1.`
`@param[in]      Arg2 Description of Arg2, which is optional.`
`@param[out]     Arg3 Description of Arg3.`
`@param[in, out] Arg4 Description of Arg4.`

`@retval EFI_SUCCESS   Description of what EFI_SUCCESS means.`
`@retval !EFI_SUCCESS  Failure.`

`**/`
`EFI_STATUS`
`EFIAPI`
`FooName (`
`IN     UINTN  Arg1,`
`IN     UINTN  Arg2 OPTIONAL,`
`OUT UINTN  *Arg3,`
`IN OUT UINTN  *Arg4`
`)`
`{`
`UINTN Local;`
`UINTN AnotherLocal;`

`...`

`for (Local = 0; Local < 5; Local++) {`
`if (Local == 2) {`
`Print (L"Local: %d (yes! 2)\n", Local);`
`} else {`
`Print (L"Local: %d\n", Local);`
`}`
`}`

`...`
`}`

H File Example:

`#ifndef _FOO_FILE_NAME_H_`
`#define _FOO_FILE_NAME_H_`

`#define FOO_MACRO(a, b) ((a) * (b))`

`#define FOO_CONSTANT  0xcafe`

`/**`
`Brief and Detailed Descriptions.`

`@param[in]      Arg1 Description of Arg1.`
`@param[in]      Arg2 Description of Arg2, which is optional.`
`@param[out]     Arg3 Description of Arg3.`
`@param[in, out] Arg4 Description of Arg4.`

`@retval EFI_SUCCESS   Description of what EFI_SUCCESS means.`
`@retval !EFI_SUCCESS  Failure.`

`**/`
`EFI_STATUS`
`EFIAPI`
`FooName (`
`IN     UINTN  Arg1,`
`IN     UINTN  Arg2 OPTIONAL,`
`OUT UINTN  *Arg3,`
`IN OUT UINTN  *Arg4`
`);`

`...`

`#endif`

## Examples

### CamelCase

Used for variables, functions and file names

Correct:

`#include "FooFileName.h"`
`...`
`VOID SuperFunction (`
`...`
`UINTN ALocalVariable;`
`UINTN UefiVersion;`

Incorrect:

`#include "foo-file_name.h"`
`...`
`VOID superFunction (`
`...`
`UINTN a_local_variable;`
`UINTN UEFIVersion;`

### UPPERCASE

Used for types and macros

Correct:

`#define FOO_MACRO(a, b) ((a) * (b))`
`typedef struct _STRUCT_NAME STRUCT_NAME;`
`struct _STRUCT_NAME {`
`...`
`};`

Incorrect:

`#define FooMacro(a, b) ((a) * (b))`
`#define Foo_Macro(a, b) ((a) * (b))`
`typedef struct _Struct_Name StructName;`

### Use UEFI types

Don't use C types directly

Correct:

`INTN   ALocalVariable;`
`UINTN  UefiVersion;`
`VOID   *Ptr;`

Incorrect:

`int           ALocalVariable;`
`unsigned int  UefiVersion;`
`void          *Ptr;`

### 2 spaces of indentation

Correct:

`if (TRUE) {`
`Print (L"Hello, world!\n");`
`}`

Incorrect:

`if (TRUE) {`
`Print (L"Hello, world!\n");`
`}`

### Always use { }

If, for, while, etc. always use { }, even when there is only one
statement

Correct:

`if (TRUE) {`
`Print (L"Hello, world!\n");`
`}`

Incorrect:

`if (TRUE)`
`Print (L"Hello, world!\n");`
