# Shell Application To Internal Command

Internal commands of the shell application are almost the same as a UEFI
Application, but they have to register with the shell binary that they
are compiled into. This means that they are made as libraries and use
the initialization routine of the library to perform this registration.
Internal commands also have a slight increase in the abilities they can
have. All of this extra requirements and features are contained within
ShellCommandLib and most only function when linked within the shell
binary image. **Your command must be linked within the shell binary to
the ShellCommandLib**

## Extra capabilities of internal commands

Internal commands have a few extra capabilities. These should be used
very judiciously since these are things that cannot always be reversed
once done. Registration (see below) is one of these extras. CommandInit
is the initialization command that you need to call at least once.

## Alias Support

Internal commands can get more direct access to the list of alias’
inside the shell. This includes getting the list of alias’ and checking
for a single alias at a time.

## Command Support

Internal commands can get a list of all commands and can get access to
the help command for a given command.

## Echo Support

Internal commands can determine and change the state of the echo
feature.

## Exit Support

Internal commands can get and set the ‘exit state’ which determine
whether the shell will exit upon completion of the current command. This
cannot be undone once set.

## Script Manipulation Support

Internal commands can parse and manipulate the script file. This means
that they can move the ‘execution point’ of the script around within the
script file.

## Mapping Support

Internal commands can add, remove, and manipulate the map names in the
shell environment. There are a lot of commands having to do with map
creation and manipulation. Most of these changes are hard to undo, but
the ShellCommandCreateInitialMappingsAndPaths function can pretty much
reset everything.

## Path and File Support

Files and Paths are manipulated vie many functions. These do not overlap
with functions in the ShellLib or the Shell protocols, but extend that
to be more feature rich. The major feature here is the conversion to
EFI_FILE_PROTOCOL from SHELL_FILE_HANDLE and back.

## Registration of a command

The registration API is as follows:

`RETURN_STATUS`
`EFIAPI`
`ShellCommandRegisterCommandName (`
`IN CONST  CHAR16                      *CommandString,`
`IN        SHELL_RUN_COMMAND           CommandHandler,`
`IN        SHELL_GET_MAN_FILENAME      GetManFileName,`
`IN        UINT32                      ShellMinSupportLevel,`
`IN CONST  CHAR16                      *ProfileName,`
`IN CONST  BOOLEAN                     CanAffectLE,`
`IN CONST  EFI_HANDLE                  HiiHandle,`
`IN CONST  EFI_STRING_ID               ManFormatHelp`
`);`

You pass into this command:

- the string you respond to (nominally the command itself)
- the function to call when this command in invoked
- the external filename for help text
- the support level your command requires
- the profile name you are part of
- whether this command can affect ‘Lasterror’ environment variable
- the HII handle that your help text is registered with
- The String Id within HII that your text is retrievable with

## Example: integrate PerformancePkg/Dp_App into ShellPkg

These steps should handle most of the required changes. Other cleanup
can be done, such as replacing PrintToken() with ShellPrintHiiEx().

Use ShellPkg/Library/UefiShellInstall1CommandsLib as your porting basis.

1. Make a new directory in ShellPkg/Library called
    DpInstall1CommandsLib.
2. Using Windows cmd.exe, change directory to ShellPkg/Library.
3. Run **copy
    UefiShellInstall1CommandsLib\UefiShellInstall1CommandsLib.\*
    DpInstall1CommandsLib\DpInstall1CommandsLib.\***
4. Edit DpInstall1CommandsLib.inf
    1. Change BASE_NAME to DpUefiShellInstall1CommandsLib
    2. Generate a new GUID for FILE_GUID using Visual Studio
        guidgen.exe or [GuidGen](https://www.guidgen.com).
    3. Change \[Sources\] to list the DP source files
        (DpUefiShellInstall1CommandsLib.c/h/uni, Dp.c, Dp.h, etc)
    4. To \[Packages\] add PerformancePkg/PerformancePkg.dec
    5. To \[LibraryClasses\] add TimerLib, PerformanceLib, and
        DxeServicesLib
    6. Change \[Protocols\] to use

        gEfiLoadedImageProtocolGuid \# ALWAYS_CONSUMED

        gEfiDriverBindingProtocolGuid \# SOMETIMES_CONSUMED

        gEfiComponentName2ProtocolGuid \# SOMETIMES_CONSUMED

        gEfiLoadedImageDevicePathProtocolGuid \# SOMETIMES_CONSUMED

        gEfiDevicePathToTextProtocolGuid \# SOMETIMES_CONSUMED
    7. To \[Pcds\] add
        gEfiMdePkgTokenSpaceGuid.PcdUefiLibMaxPrintBufferSize
    8. Change \[Guids\] to use gDpShellInstall1HiiGuid
5. Edit DpInstall1CommandsLib.uni
    1. Change all instances of BCFG to DP, keeping the case of the
        changed item, e.g.
        1. Change STR_GET_HELP_BCFG to STR_GET_HELP_DP
        2. Change .TH bcfg to .TH dp
    2. Change all help text to reflect help text in DpStrings.uni.
6. Edit DpInstall1CommandsLib.h
    1. Change all instances of BCFG to DP, keeping the case of the
        changed item.
    2. Change gShellInstall1HiiHandle to gDpShellInstall1HiiHandle.
7. Edit DpInstall1CommandsLib.c
    1. Change all instances of BCFG to DP, keeping the case of the
        changed item.
    2. Change gShellInstall1HiiHandle to gDpShellInstall1HiiHandle.
    3. Include DpUefiShellInstall1CommandsLib.h
    4. Add Dp to the start of each function name so they start with
        DpShell.
    5. Change all instances of gShellInstall1HiiHandle to
        gDpShellInstall1HiiHandle.
    6. In DpShellInstall1CommandsLibConstructor, this comment should be
        above HiiAddPackages().

        //

        // 3rd parameter 'HII strings array' must be name of .uni
        strings file followed by 'Strings', e.g. mycommands.uni must be

        // specified as 'mycommandsStrings' because the build Autogen
        process defines this as a string array for the strings in your

        // .uni file. Examine your Build folder under your package's
        DEBUG folder and you will find it defined in a xxxStrDefs.h
        file.

        //
8. In DpUtilities.c, DpTrace.c, DpProfile.c, DpInternal.h change all
    instances of gHiiHandle to gDpShellInstall1HiiHandle.
9. In Dp.c do the following:
    1. Change all instances of gHiiHandle to gDpShellInstall1HiiHandle.
    2. Add the following:

        \#include "DpUefiShellInstall1CommandsLib.h"

        \#include \<Guid/GlobalVariable.h\>

        \#include \<Library/PrintLib.h\>

        \#include \<Library/HandleParsingLib.h\>

        \#include \<Library/DevicePathLib.h\>
    3. Change InitializeDp to ShellCommandRunDpInstall.
    4. Replace the code between GetPerformanceCounter() and
        ShellCommandLineParse() with the following:

        //

        // initialize the shell lib (we must be in non-auto-init...)

        //

        Status = ShellInitialize();

        ASSERT_EFI_ERROR(Status);

        Status = CommandInit();

        ASSERT_EFI_ERROR(Status);
10. Edit ShellPkg/Include/Guid/ShellLibHiiGuid.h and do the following:
    1. Add the GUID in DpInstall1CommandsLib.inf and call it
        SHELL_DP_INSTALL1_HII_GUID.
    2. Add 'extern EFI_GUID gDpShellInstall1HiiGuid'.
11. Edit ShellPkg/ShellPkg.dec and add the GUID in
    DpInstall1CommandsLib.inf and call it gDpShellInstall1HiiGuid.
12. Edit ShellPkg/ShellPkg.dsc and do the following:
    1. Add
        ShellPkg/Library/DpInstall1CommandsLib/DpUefiShellInstall1CommandsLib.inf
        as a NULL library instance to Shell.inf {} section.
    2. Add the appropriate TimerLib and PerformanceLib drivers to the
        \[LibraryClasses\] section.
    3. Add the following library instances to \[LibraryClasses.common\]

        IoLib\|MdePkg/Library/BaseIoLibIntrinsic/BaseIoLibIntrinsic.inf

        PciLib\|MdePkg/Library/BasePciLibCf8/BasePciLibCf8.inf

        PciCf8Lib\|MdePkg/Library/BasePciCf8Lib/BasePciCf8Lib.inf

        DxeServicesLib\|MdePkg/Library/DxeServicesLib/DxeServicesLib.inf
