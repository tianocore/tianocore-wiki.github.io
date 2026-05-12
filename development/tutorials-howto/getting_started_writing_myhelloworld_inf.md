# Getting Started Writing MyHelloWOrld.Inf

Example of MyHelloWorld.inf

Please check for the latest version of the
[EDK II Specifications](../../reference/specs-standards/edk_ii_specifications.md)

Note: Copy and paste the GUID from : [https://www.guidgen.com/](https://www.guidgen.com/) for the
**FILE_GUID** in the example below

    ## @file
    #  Brief Description of UEFI MyHelloWorld
    #
    #  Detailed Description of UEFI MyWizardDriver
    #
    #  Copyright for UEFI  MyHelloWorld
    #
    #  License for UEFI  MyHelloWorld
    #
    ##

    [Defines]
      INF_VERSION                    = 1.25
      BASE_NAME                      = MyHelloWorld
      FILE_GUID                      = #Copy and paste the GUID from https://www.guidgen.com/ here
      MODULE_TYPE                    = UEFI_APPLICATION
      VERSION_STRING                 = 1.0
      ENTRY_POINT                    = UefiMain
    #
    # The following information is for reference only and not required by the build tools.
    #
    #  VALID_ARCHITECTURES           = IA32 X64 IPF EBC Etc...
    #

    [Sources]
      MyHelloWorld.c

    [Packages]
      MdePkg/MdePkg.dec

    [LibraryClasses]
      UefiApplicationEntryPoint
      UefiLib

    [Guids]

    [Ppis]

    [Protocols]

    [FeaturePcd]

    [Pcd]
