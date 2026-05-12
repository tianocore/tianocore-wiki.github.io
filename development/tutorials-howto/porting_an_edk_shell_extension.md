# Porting An EDK Shell Extension

The goal of this section is to enable an engineer to easily take an EDK
Shell Extension and move that to an EDK II UEFI Shell 2.0 application.
This means primarily removing and replacing 3 types of references.

- EDK Shell protocols usage
- EDK Shell library usage
- EDK library usage and ‘standard’ EDK globals

The last of these three items is not always present whereas the first
two are almost always present.

## Eliminating the EDK Shell Protocols

There are 3 protocols directly associated with the EDK Shell. The
protocols are:

- EFI_SHELL_ENVIRONMENT
- EFI_SHELL_ENVIRONMENT2
- EFI_SHELL_INTERFACE

The first protocol (EFI_SHELL_ENVIRONMENT) is a subset of the second one
(EFI_SHELL_ENVIRONMENT2) and will not have a separate section.

### Eliminating EFI_SHELL_ENVIRONMENT2

#### Execute

Use EFI_SHELL_PROTOCOL-\>Execute. This function does not exactly match,
but has almost all of the same features. The major change is that the
UEFI Shell Spec 2.0 does not allow for the running of internal shell
commands through this function.

#### GetEnv

Use EFI_SHELL_PROTOCOL-\>GetEnv.

#### GetMap

This concept does not exist in the new shell. Use Shell Environment
instead of shell maps.

#### AddCmd

This concept does not exist in the new shell. There is no way to
dynamically add a command to the shell.

#### AddProt

This concept does not exist in the new shell. There is no way to
dynamically add protocol information to the shell. You can look at the
HandleParsingLib for some similar functionality.

#### GetProt

This concept does not exist in the new shell. There is no way to query
protocol information in the shell. You can look at the HandleParsingLib
for some similar functionality.

#### CurDir

Use EFI_SHELL_PROTOCOL-\> GetCurDir.

#### FileMetaArg

Use EFI_SHELL_PROTOCOL-\> OpenFileList

#### FreeFileList

Use EFI_SHELL_PROTOCOL-\> FreeFileList

#### NewShell

This concept does not exist in the new shell. You can spawn a new shell
via the Execute function.

#### BatchIsActive

Use EFI_SHELL_PROTOCOL-\> BatchIsActive

#### FreeResources

This concept does not exist in the new shell.

#### EnablePageBreak

Use EFI_SHELL_PROTOCOL-\> EnablePageBreak (Note that this is
automatically controlled via ShellLib command line parsing functions)

#### DisablePageBreak

Use EFI_SHELL_PROTOCOL-\> DisablePageBreak

#### GetPageBreak

Use EFI_SHELL_PROTOCOL-\> GetPageBreak

#### SetKeyFilter

This concept is not part of the UEFI Shell. Key filtering must be done
either manually or via the ShellLib with the functions
ShellPromptForResponse and ShellPromptForResponseHii.

#### GetKeyFilter

This concept is not part of the UEFI Shell. Key filtering must be done
either manually or via the ShellLib with the functions
ShellPromptForResponse and ShellPromptForResponseHii.

#### GetExecutionBreak

This concept has changed in the UEFI Shell. There is a event that you
can query EFI_SHELL_PROTOCOL-\> ExecutionBreak. There is also a function
in the ShellLib that helps (ShellGetExecutionBreakFlag).

#### IncrementShellNestingLevel

This concept does not exist in the new shell. The nesting level cannot
be controlled outside the shell.

#### DecrementShellNestingLevel

This concept does not exist in the new shell. The nesting level cannot
be controlled outside the shell.

#### IsRootShell

Use EFI_SHELL_PROTOCOL-\>IsRootShell.

#### CloseConsoleProxy

This concept does not exist in the new shell. The consoles cannot be
controlled outside the shell.

### HandleEnumerator (function group)

This concept does not exist in the new shell. There is no way to query
protocol information in the shell. You can look at the HandleParsingLib
for some similar functionality.

### ProtocolInfoEnumerator (function group)

This concept does not exist in the new shell. There is no way to query
protocol information in the shell. You can look at the HandleParsingLib
for some similar functionality.

#### GetDeviceName

Use EFI_SHELL_PROTOCOL-\> GetDeviceName

#### GetShellMode

This concept does not exist in the UEFI Shell. Shell support level and
shell profiles present can be retrieved from environment variables.

#### NameToPath

Use EFI_SHELL_PROTOCOL-\> GetDevicePathFromFilePath

#### GetFsName

Use EFI_SHELL_PROTOCOL-\> GetMapFromDevicePath

#### FileMetaArgNoWildcard

Use EFI_SHELL_PROTOCOL-\> OpenFileByName

#### DelDupFileArg

Use EFI_SHELL_PROTOCOL-\> RemoveDupInFileList

#### GetFsDevicePath

Use EFI_SHELL_PROTOCOL-\> GetDevicePathFromMap

### Eliminating EFI_SHELL_INTERFACE

#### ImageHandle

Use ImageHandle from entry point. Most apps have gImageHandle global
variable linked in from libraries.

#### Info

If you want EFI_LOADED_IMAGE_PROTOCOL you can get it manually.

#### Argv

Use EFI_SHELL_PARAMETERS_PROTOCOL-\>Argv

#### Argc

Use EFI_SHELL_PARAMETERS_PROTOCOL-\>Argv

#### RedirArgv

This concept does not exist in the new shell. Redirection is handled
before applications are launched.

#### RedirArgc

This concept does not exist in the new shell. Redirection is handled
before applications are launched.

#### StdIn

Use EFI_SHELL_PARAMETERS_PROTOCOL-\>StdIn

#### StdOut

Use EFI_SHELL_PARAMETERS_PROTOCOL-\>StdOut

#### StdErr

Use EFI_SHELL_PARAMETERS_PROTOCOL-\>StdErr

#### ArgInfo

This concept does not exist in the new shell.

#### EchoOn

This concept is not checkable in the new shell. The shell internally
controls the echo state.

## Eliminating Shell library functions

### Eliminating common EfiShellLib functions

Due to the many different versions of the shell library and the many
possible functions, only the most common functions will be covered here.
If you find a function is not covered please report that as an
enhancement request.

#### EFI_SHELL_APP_INIT, other initialization macro, or any initialization function

This should not be required. The EDKII ShellLib will automatically
initialize itself. Only when building a shell internal command this is
not true and the manual initialization must be called.

#### EFI_SHELL_STR_INIT, LibInitializeStrings, or any string function

If you need some strings put into HII it is expected that you will
handle this yourself. Please refer to the HiiLib library class for doing
this.

#### Math Functions

- such as LShiftU64, RShiftU64, MultU64x32, DivU64x32, etc...

All of these should be changed to EDK II library class BaseLib math
functions.

#### GetBestLanguage

This function exists in the EDK II Library class UefiLib.

#### LibGetDriverName

This function can be called directly via COMPONENT_NAME_PROTOCOL.

#### Printing functions

##### PrintAt

This function is replaced by the EDK II ShellLib functions ShellPrintEx
and ShellPrintHiiEx. You decide which to use based on whether the format
string to be printed is contained in HII or not.

##### Print

This function exists in the EDK II Library class UefiLib.

##### Output

This function is replaced by the Print() function.

##### SPrint

This function is in the process of being ported to EDK II.

##### PoolPrint

This function is in the process of being ported to EDK II.

##### PrintToken

This function is replaced by the EDK II ShellLib function
ShellPrintHiiEx.

##### CatPrint

This function is replaced by the EDK II ShellLib function
ShellPrintHiiEx.

#### Command Line Variable functions

The structure that you use for storing potential parameters is changed.
Please see the new structure in the EDK II ShellLib.

##### GetNextArg, GetFirstArg, GetFirstFlag

These are all deprecated. The order of flags/arguments is not verified
in the automatic parsing. For command lines where there are items passed
in, but these items are not associated with a flag (for example,
“command 0xff 0xaa” there is the EDK II ShellLib function
ShellCommandLineGetRawValue.

##### LibCheckVarFreeVarList

Use the EDK II ShellLib function ShellCommandLineFreeVarList.

##### LibCheckVarGetFlag

This function is divided into 2 EDK II ShellLib functions. The first is
ShellCommandLineGetFlag and is used to get whether a flag was present on
the command line. The second is ShellCommandLineGetValue and is used to
get the value that followed a flag on the command line (if there was a
following value).

##### LibCheckVariables

Use the EDK II ShellLib function ShellCommandLineParseEx.

##### LibCheckRedirVariables

This concept does not exist in the new shell. Redirection is handled
before applications are launched.

##### LibGetStdRedirFilename

This concept does not exist in the new shell. Redirection is handled
before applications are launched.

##### LibGetErrRedirFilename

This concept does not exist in the new shell. Redirection is handled
before applications are launched.

## Eliminating EDK library usage and ‘standard’ EDK globals

### Eliminating EDK library usage

All EDK library usage must be replaced.

### Eliminating ‘standard’ EDK globals

#### RT

If your application is linked to the UefiRuntimeServicesTableLib then
you will get a global called gRT. This is the replacement for RT.

#### BS

If your application is linked to the UefiBootServicesTableLib then you
will get a global called gBS. This is the replacement for BS.

#### gSPP

If your application is linked to the ShellLib then you will get a global
called gEfiShellParametersProtocol. This is the replacement for gSPP.

[ShellLib
header](https://sourceforge.net/p/edk2/code/HEAD/tree/trunk/edk2/ShellPkg/Include/Library/ShellLib.h)

#### gSP

If your application is linked to the ShellLib then you will get a global
called gEfiShellProtocol. This is the replacement for gSP.

[ShellLib
header](https://sourceforge.net/p/edk2/code/HEAD/tree/trunk/edk2/ShellPkg/Include/Library/ShellLib.h)
