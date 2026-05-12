# ArmPkg Runtime

## How to write a UEFI Runtime driver

### Implementation Rules

- Ensure your driver has been declared with 'MODULE_TYPE =
  DXE_RUNTIME_DRIVER' in its INF file.

That would load the code and data sections into UEFI memory regions
marked as 'Runtime' regions (EfiRuntimeServicesCode and
EfiRuntimeServicesData)

- Allocate all the persistent data into runtime space (ie: use
  AllocateRuntimePool() instead of AllocatePool())
- Do not allocate memory in code called after ExitBootServices().
- Do not access BootServices API during Runtime mode - or protect the
  code:

      if (!EfiAtRuntime ()) {
        // Raise TPL to TPL_HIGH to stop anyone from interrupting us.
        OriginalTPL = gBS->RaiseTPL (TPL_HIGH_LEVEL);
      }

- Declare the controller memory region as Runtime Memory Mapped IO:

      // Declare the controller as EFI_MEMORY_RUNTIME
      Status = gDS->AddMemorySpace (
                      EfiGcdMemoryTypeMemoryMappedIo,
                      mControllerRegionBase, mControllerRegionSize,
                      EFI_MEMORY_UC | EFI_MEMORY_RUNTIME
                      );
      ASSERT_EFI_ERROR (Status);

Status = gDS->SetMemorySpaceAttributes (mControllerRegionBase, mControllerRegionSize, EFI_MEMORY_UC |
EFI_MEMORY_RUNTIME);
      ASSERT_EFI_ERROR (Status);

- Do not hardcode base addresses in code called after
  ExitBootServices(). These addresses might need to be fixed up in
  Operating System Virtual Memory view.
- Fixup all the controller and dynamic addresses invoked during Runtime
  mode. Example:

    VOID
    EFIAPI
    MyDriverVirtualNotifyEvent (
      IN EFI_EVENT        Event,
      IN VOID             *Context
      )
    {
      EfiConvertPointer (0x0, (VOID**)&mMyControllerBase);

      // Convert allocated buffer
      EfiConvertPointer (0x0, (VOID**)&mMyDriverInstance->Buffer);

      // Convert BlockIo protocol
      EfiConvertPointer (0x0, (VOID**)&mMyDriverInstance->BlockIoProtocol.FlushBlocks);
      EfiConvertPointer (0x0, (VOID**)&mMyDriverInstance->BlockIoProtocol.ReadBlocks);
      EfiConvertPointer (0x0, (VOID**)&mMyDriverInstance->BlockIoProtocol.Reset);
      EfiConvertPointer (0x0, (VOID**)&mMyDriverInstance->BlockIoProtocol.WriteBlocks);

      return;
    }

    DriverEntryPoint () {
      (...)
      mMyDriverInstance->Buffer = AllocateRuntimePool (BUFFER_SIZE);

      //
      // Register for the virtual address change event
      //
      Status = gBS->CreateEventEx (
                      EVT_NOTIFY_SIGNAL,
                      TPL_NOTIFY,
                      MyDriverVirtualNotifyEvent,
                      NULL,
                      &gEfiEventVirtualAddressChangeGuid,
                      &mMyDriverVirtualAddrChangeEvent
                      );
      ASSERT_EFI_ERROR (Status);

      (...)
    }

### Validation

#### UEFI Shell

Confirm the runtime regions are present by using the command 'memmap' in
the EFI Shell.

**Note:** The attribute '8000000000000000' means runtime memory
region

    UEFI v2.40 (ARM Fixed Virtual Platform EFI Apr 11 2014 11:55:10, 0x00000000)
    Shell> memmap
    Type      Start            End              #pages             Attributes
    Available 0000000080000000-0000000087FFFFFF 0000000000008000 000000000000000F
    Available 000000008C000000-00000000FFAB0FFF 0000000000073AB1 000000000000000F
    BS_Data   00000000FFAB1000-00000000FFFFFFFF 000000000000054F 000000000000000F
    Available 0000000880000000-00000008FADC2FFF 000000000007ADC3 000000000000000F
    LoaderCode 00000008FADC3000-00000008FAEBAFFF 00000000000000F8 000000000000000F
    BS_Code   00000008FAEBB000-00000008FAFB2FFF 00000000000000F8 000000000000000F
    RT_Code   00000008FAFB3000-00000008FAFBEFFF 000000000000000C 800000000000000F <-- Runtime
    RT_Data   00000008FAFBF000-00000008FAFD7FFF 0000000000000019 800000000000000F <-- Runtime
    RT_Code   00000008FAFD8000-00000008FAFF4FFF 000000000000001D 800000000000000F <-- Runtime
    Available 00000008FAFF5000-00000008FC6DAFFF 00000000000016E6 000000000000000F
    BS_Data   00000008FC6DB000-00000008FC74DFFF 0000000000000073 000000000000000F
    Available 00000008FC74E000-00000008FC7C0FFF 0000000000000073 000000000000000F
    BS_Data   00000008FC7C1000-00000008FC8B8FFF 00000000000000F8 000000000000000F
    Available 00000008FC8B9000-00000008FC8D5FFF 000000000000001D 000000000000000F
    BS_Data   00000008FC8D6000-00000008FC906FFF 0000000000000031 000000000000000F
    Available 00000008FC907000-00000008FC91EFFF 0000000000000018 000000000000000F
    BS_Data   00000008FC91F000-00000008FC95CFFF 000000000000003E 000000000000000F
    Available 00000008FC95D000-00000008FC973FFF 0000000000000017 000000000000000F
    BS_Data   00000008FC974000-00000008FFE28FFF 00000000000034B5 000000000000000F
    Available 00000008FFE29000-00000008FFE91FFF 0000000000000069 000000000000000F
    BS_Code   00000008FFE92000-00000008FFFB8FFF 0000000000000127 000000000000000F
    RT_Code   00000008FFFB9000-00000008FFFCCFFF 0000000000000014 800000000000000F <-- Runtime
    RT_Data   00000008FFFCD000-00000008FFFFEFFF 0000000000000032 800000000000000F <-- Runtime
    BS_Data   00000008FFFFF000-00000008FFFFFFFF 0000000000000001 000000000000000F
    MMIO      000000000C000000-000000000FFEFFFF 0000000000003FF0 8000000000000001 <-- Runtime (NOR Flash)
    MMIO      000000001C170000-000000001C170FFF 0000000000000001 8000000000000001 <-- Runtime (RTC controller)
      Reserved  :          0 Pages (0)
      LoaderCode:        248 Pages (1,015,808)
      LoaderData:          0 Pages (0)
      BS_Code   :        543 Pages (2,224,128)
      BS_Data   :     15,327 Pages (62,779,392)
      RT_Code   :         61 Pages (249,856)
      RT_Data   :         75 Pages (307,200)
      ACPI Recl :          0 Pages (0)
      ACPI NVS  :          0 Pages (0)
      MMIO      :     16,369 Pages (67,047,424)
      Available :  1,015,938 Pages (4,161,282,048)
    Total Memory: 4095 MB (4,294,905,856 Bytes)
    Shell>

#### Linux

By passing the argument 'uefi_debug', the Linux kernel dumps the UEFI
information. Example:

    ```text
        EFI stub: Booting Linux Kernel...
        Initializing cgroup subsys cpu
    Linux version 3.14.0-next-20140402+ (gcc version 4.8.3 20131111 (prerelease) (crosstool-NG linaro-1.13.1-4.8-2013.11 -
    Linaro GCC 2013.10) ) #1 SMP PREEMPT Fri Apr 11 15:39:25 BST 2014
    CPU: AArch64 Processor [410fd0f0] revision 0
    bootconsole [earlycon0] enabled
    efi: Getting parameters from FDT:
    efi:   System Table: 0x00000008ffffef18
    efi:   MemMap Address: 0x00000008fa366018
    efi:   MemMap Size: 0x00000930
    efi:   MemMap Desc. Size: 0x00000030
    efi:   MemMap Desc. Version: 0x00000001
    EFI v2.40 by ARM Fixed Virtual Platform EFI Apr 11 2014 11:55:10
    efi:
    Processing EFI memory map:
      0x000080000000-0x000080000fff [Loader Data]
      0x000080001000-0x00008007ffff [Conventional Memory]
      0x000080080000-0x000080655fff [Loader Data]
      0x000080656000-0x000087ffffff [Conventional Memory]
      0x00008c000000-0x00009fdfffff [Conventional Memory]
      0x00009fe00000-0x00009fe03fff [Loader Data]
      0x00009fe04000-0x0000ffab0fff [Conventional Memory]
      0x0000ffab1000-0x0000ffffffff [Boot Data]*
      0x000880000000-0x0008fa365fff [Conventional Memory]
      0x0008fa366000-0x0008fa366fff [Loader Data]
      0x0008fa367000-0x0008fa910fff [Loader Code]
      0x0008fa911000-0x0008fafb2fff [Boot Code]*
      0x0008fafb3000-0x0008fafbefff [Runtime Code]*
      0x0008fafbf000-0x0008fafd7fff [Runtime Data]*
      0x0008fafd8000-0x0008faff4fff [Runtime Code]*
      0x0008faff5000-0x0008fc63cfff [Conventional Memory]
      0x0008fc63d000-0x0008fc74dfff [Boot Data]*
      0x0008fc74e000-0x0008fc7c0fff [Conventional Memory]
      0x0008fc7c1000-0x0008fc8b8fff [Boot Data]*
      0x0008fc8b9000-0x0008fc8d5fff [Conventional Memory]
      0x0008fc8d6000-0x0008fc906fff [Boot Data]*
      0x0008fc907000-0x0008fc91efff [Conventional Memory]
      0x0008fc91f000-0x0008fc937fff [Boot Data]*
      0x0008fc938000-0x0008fc975fff [Conventional Memory]
      0x0008fc976000-0x0008fc981fff [Boot Data]*
      0x0008fc982000-0x0008fc98efff [Conventional Memory]
      0x0008fc98f000-0x0008fc991fff [Boot Data]*
      0x0008fc992000-0x0008fc9b0fff [Conventional Memory]
      0x0008fc9b1000-0x0008fce31fff [Boot Data]*
      0x0008fce32000-0x0008fce80fff [Conventional Memory]
      0x0008fce81000-0x0008ff682fff [Boot Data]*
      0x0008ff683000-0x0008ff685fff [Conventional Memory]
      0x0008ff686000-0x0008ff686fff [Boot Data]*
      0x0008ff687000-0x0008ff698fff [Conventional Memory]
      0x0008ff699000-0x0008ff699fff [Boot Data]*
      0x0008ff69a000-0x0008ff69dfff [Conventional Memory]
      0x0008ff69e000-0x0008ff69efff [Boot Data]*
      0x0008ff69f000-0x0008ff69ffff [Conventional Memory]
      0x0008ff6a0000-0x0008ff6c4fff [Boot Data]*
      0x0008ff6c5000-0x0008ff6cafff [Conventional Memory]
      0x0008ff6cb000-0x0008ffe28fff [Boot Data]*
      0x0008ffe29000-0x0008ffe8efff [Conventional Memory]
      0x0008ffe8f000-0x0008ffe91fff [Loader Data]
      0x0008ffe92000-0x0008fffb8fff [Boot Code]*
      0x0008fffb9000-0x0008fffccfff [Runtime Code]*
      0x0008fffcd000-0x0008ffffefff [Runtime Data]*
      0x0008fffff000-0x0008ffffffff [Boot Data]*
      0x00000c000000-0x00000ffeffff [Memory Mapped I/O]
      0x00001c170000-0x00001c170fff [Memory Mapped I/O]
    (...)

    Kernel command line: dtb=fvp-base-gicv2-psci.dtb console=ttyAMA0 earlyprintk=pl011,0x1c090000 debug uefi_debug
    root=/dev/vda2 rw
        (...)
        Remapping and enabling EFI services.
          EFI remap 0x0008fafb3000 => ffffffc87afb3000
          EFI remap 0x0008fafbf000 => ffffffc87afbf000
          EFI remap 0x0008fafd8000 => ffffffc87afd8000
          EFI remap 0x0008fffb9000 => ffffffc87ffb9000
          EFI remap 0x0008fffcd000 => ffffffc87ffcd000
          EFI remap 0x00000c000000 => ffffff8000080000
          EFI remap 0x00001c170000 => ffffff8000012000
          EFI freeing: 0x0000ffab1000-0x0000ffffffff
          EFI freeing: 0x0008fa911000-0x0008fafb2fff
          EFI freeing: 0x0008fc63d000-0x0008fc74dfff
          EFI freeing: 0x0008fc7c1000-0x0008fc8b8fff
          EFI freeing: 0x0008fc8d6000-0x0008fc906fff
          EFI freeing: 0x0008fc91f000-0x0008fc937fff
          EFI freeing: 0x0008fc976000-0x0008fc981fff
          EFI freeing: 0x0008fc98f000-0x0008fc991fff
          EFI freeing: 0x0008fc9b1000-0x0008fce31fff
          EFI freeing: 0x0008fce81000-0x0008ff682fff
          EFI freeing: 0x0008ff686000-0x0008ff686fff
          EFI freeing: 0x0008ff699000-0x0008ff699fff
          EFI freeing: 0x0008ff69e000-0x0008ff69efff
          EFI freeing: 0x0008ff6a0000-0x0008ff6c4fff
          EFI freeing: 0x0008ff6cb000-0x0008ffe28fff
          EFI freeing: 0x0008ffe92000-0x0008fffb8fff
          EFI freeing: 0x0008fffff000-0x0008ffffffff
        Freed 0x4384000 bytes of EFI boot services memory
        (...)
    ```

Accessing the UEFI variables from the Linux terminal:

    ```text
    root@genericarmv8:~# efibootmgr -d /dev/vda
    BootCurrent: 0003
    Timeout: 5 seconds
    BootOrder: 0003
    Boot0000* Linux from SemiHosting
    Boot0003  EFI Stub
    Boot0005* grub
    root@genericarmv8:~#
    ```

### Debugging

To debug UEFI Runtime services, you could have a look at the
'Validation' section to ensure your UEFI Services behave as expected.

If the UEFI Runtime Services are not implemented correctly, your
Operating System would crash when accessing them.

#### Use Case 1: EfiConvertPointer()

1\. Trigger the crash in Linux. Keep the failing virtual address and the
values of the PC and LR registers.
![](RuntimeUseCase-1.PNG "RuntimeUseCase-1.PNG")

- Failing Virtual Adress: 0xffaae850
- PC: 0xffffffc0031c473c
- LR: 0xffffffc0031c4710

2\. Get the remapped UEFI Runtime regions of the Linux Virtual Memory
map ![](RuntimeUseCase-2.PNG "RuntimeUseCase-2.PNG") ... and find out
which region the failing values are part of:

- 0x0000831b7000 =\> 0xffffffc0031b7000
- 0x0000ffa99000 =\> 0xffffffc07fa99000

And calculate the offset (it is likely the offsets are the same for each
mapping).
In our case the offste is 0xFFFFFFBF80000000 for both ranges.

From this offset, we can get the corresponding PC value into the UEFI
space: 0x831C473C

3\. Restart UEFI and load the symbols.
Set a breakpoint (or go to the PC location)
![](RuntimeUseCase-3.PNG "RuntimeUseCase-3.PNG") From this window, we
can see the 'Fvb' variable is at 0xffaae840.
'Fvb-\>GetPhysicalAddress()' is actually at 0xffaae850.
We can conclude that the Linux kernel was still accessing the address of
Fvb-\>GetPhysicalAddress in the UEFI memory view.

The fix is to convert this address when the Operating System notifies
the change of the Virtual Memory map to the UEFI Runtime Services:

    VOID
    EFIAPI
    NorFlashVirtualNotifyEvent (
      IN EFI_EVENT        Event,
      IN VOID             *Context
      )
    {
      (..)
      EfiConvertPointer (0x0, (VOID**)&mNorFlashInstance->FvbProtocol.GetPhysicalAddress);
      (..)
    }

#### Use Case 2: Boot Service API

1\. Get the information from the crash
![](RuntimeUseCase-4.PNG "RuntimeUseCase-4.PNG")

2\. Restart UEFI and load the symbols.
Set a breakpoint (or go to the PC location)
![](RuntimeUseCase-5.PNG "RuntimeUseCase-5.PNG")

**Conclusion:** AllocatePool (UEFI Boot Services function) is invoked
during the UEFI Runtime phase (after gBS-\>ExitBootServices() is
called).
It is illegal!

#### Use Case 3: Direct access to hardware registers

1\. Get the information from the crash
![](RuntimeUseCase-6.PNG "RuntimeUseCase-6.PNG")

0x0c00_0000 is actually the NOR Flash address (see:
[https://developer.arm.com/documentation/dui0677/c/BBACIHDC](https://developer.arm.com/documentation/dui0677/c/BBACIHDC)).
The UEFI Runtime NOR Flash driver accessed the hardware region without
updating the base address into the Linux Virtual Memory Map.

To identify which part of the driver is accessing the hardware register,
we load the symbols into the Linux Memory map.
In UEFI Memory map:

Add symbols of
/home/olimar01/tianocore/Build/ArmVExpress-FVP-AArch64/DEBUG_GCC48/AARCH64/ArmPlatformPkg/Drivers/NorFlashDxe/NorFlashDxe/DEBUG/ArmVeNorFlashDxe.dll
at 0x831d4260

After converting the UEFI address into the Linux Memory Map (and force
the symbols to be loaded in EL1 Non-Secure world):

add-symbol-file
/home/olimar01/tianocore/Build/ArmVExpress-FVP-AArch64/DEBUG_GCC48/AARCH64/ArmPlatformPkg/Drivers/NorFlashDxe/NorFlashDxe/DEBUG/ArmVeNorFlashDxe.dll
EL1N:0xFFFFFFC0031D4260

2\. Restart the environment. Load the symbols and add the breakpoint:

add-symbol-file
/home/olimar01/tianocore/Build/ArmVExpress-FVP-AArch64/DEBUG_GCC48/AARCH64/ArmPlatformPkg/Drivers/NorFlashDxe/NorFlashDxe/DEBUG/ArmVeNorFlashDxe.dll
EL1N:0xFFFFFFC0031D4260
    hbreak -p EL1N:0xffffffc0031d8e14

![](RuntimeUseCase-7.PNG)
*RuntimeUseCase-7.PNG*
**Conclusion:** Fixing up Instance-\>DeviceAddress (physical address
of the NOR Flash device) when the virtual memory map is changed at
Runtime should fix the issue.

### Tuning

We saw earlier there were multiple UEFI Data and Code Runtime regions in
the EFI Shell.

    UEFI v2.40 (ARM Fixed Virtual Platform EFI Apr 11 2014 11:55:10, 0x00000000)
    Shell> memmap
    Type      Start            End              #pages             Attributes
    Available 0000000080000000-0000000087FFFFFF 0000000000008000 000000000000000F
    Available 000000008C000000-00000000FFAB0FFF 0000000000073AB1 000000000000000F
    BS_Data   00000000FFAB1000-00000000FFFFFFFF 000000000000054F 000000000000000F
    Available 0000000880000000-00000008FADC2FFF 000000000007ADC3 000000000000000F
    LoaderCode 00000008FADC3000-00000008FAEBAFFF 00000000000000F8 000000000000000F
    BS_Code   00000008FAEBB000-00000008FAFB2FFF 00000000000000F8 000000000000000F
    RT_Code   00000008FAFB3000-00000008FAFBEFFF 000000000000000C 800000000000000F <-- Runtime
    RT_Data   00000008FAFBF000-00000008FAFD7FFF 0000000000000019 800000000000000F <-- Runtime
    RT_Code   00000008FAFD8000-00000008FAFF4FFF 000000000000001D 800000000000000F <-- Runtime
    Available 00000008FAFF5000-00000008FC6DAFFF 00000000000016E6 000000000000000F
    BS_Data   00000008FC6DB000-00000008FC74DFFF 0000000000000073 000000000000000F
    Available 00000008FC74E000-00000008FC7C0FFF 0000000000000073 000000000000000F
    BS_Data   00000008FC7C1000-00000008FC8B8FFF 00000000000000F8 000000000000000F
    Available 00000008FC8B9000-00000008FC8D5FFF 000000000000001D 000000000000000F
    BS_Data   00000008FC8D6000-00000008FC906FFF 0000000000000031 000000000000000F
    Available 00000008FC907000-00000008FC91EFFF 0000000000000018 000000000000000F
    BS_Data   00000008FC91F000-00000008FC95CFFF 000000000000003E 000000000000000F
    Available 00000008FC95D000-00000008FC973FFF 0000000000000017 000000000000000F
    BS_Data   00000008FC974000-00000008FFE28FFF 00000000000034B5 000000000000000F
    Available 00000008FFE29000-00000008FFE91FFF 0000000000000069 000000000000000F
    BS_Code   00000008FFE92000-00000008FFFB8FFF 0000000000000127 000000000000000F
    RT_Code   00000008FFFB9000-00000008FFFCCFFF 0000000000000014 800000000000000F <-- Runtime
    RT_Data   00000008FFFCD000-00000008FFFFEFFF 0000000000000032 800000000000000F <-- Runtime
    BS_Data   00000008FFFFF000-00000008FFFFFFFF 0000000000000001 000000000000000F
    MMIO      000000000C000000-000000000FFEFFFF 0000000000003FF0 8000000000000001 <-- Runtime (NOR Flash)
    MMIO      000000001C170000-000000001C170FFF 0000000000000001 8000000000000001 <-- Runtime (RTC controller)
      Reserved  :          0 Pages (0)
      LoaderCode:        248 Pages (1,015,808)
      LoaderData:          0 Pages (0)
      BS_Code   :        543 Pages (2,224,128)
      BS_Data   :     15,327 Pages (62,779,392)
      RT_Code   :         61 Pages (249,856) <-- Runtime Code regions
      RT_Data   :         75 Pages (307,200) <-- Runtime Data regions
      ACPI Recl :          0 Pages (0)
      ACPI NVS  :          0 Pages (0)
      MMIO      :     16,369 Pages (67,047,424)
      Available :  1,015,938 Pages (4,161,282,048)
    Total Memory: 4095 MB (4,294,905,856 Bytes)
    Shell>

These fragmented UEFI regions would still appear fragmented when booting
Linux:

    Remapping and enabling EFI services.
      EFI remap 0x0008fafb3000 => ffffffc87afb3000
      EFI remap 0x0008fafbf000 => ffffffc87afbf000
      EFI remap 0x0008fafd8000 => ffffffc87afd8000
      EFI remap 0x0008fffb9000 => ffffffc87ffb9000
      EFI remap 0x0008fffcd000 => ffffffc87ffcd000
      EFI remap 0x00000c000000 => ffffff8000080000
      EFI remap 0x00001c170000 => ffffff8000012000

These regions can get unify by declaring EFI_MEMORY_TYPE_INFORMATION
using the gEfiMemoryTypeInformationGuid HOB.
Support for this HOB already exists in the ARM Platform framework, you
only need to:

- Set PcdPrePiProduceMemoryTypeInformationHob to TRUE
- Define the number of pages for the Runtime Data and Code regions:

    --- a/ArmPlatformPkg/ArmVExpressPkg/ArmVExpress.dsc.inc
    +++ b/ArmPlatformPkg/ArmVExpressPkg/ArmVExpress.dsc.inc
    @@ -318,8 +318,8 @@
       gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiACPIReclaimMemory|0
       gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiACPIMemoryNVS|0
       gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiReservedMemoryType|0
  - gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiRuntimeServicesData|50
  - gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiRuntimeServicesCode|20
  - gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiRuntimeServicesData|80
  - gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiRuntimeServicesCode|65
       gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiBootServicesCode|400
       gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiBootServicesData|20000
       gEmbeddedTokenSpaceGuid.PcdMemoryTypeEfiLoaderCode|20

After rebuilding the UEFI firmware, your regions should now be unified:

    Shell> memmap
    Type      Start            End              #pages             Attributes
    Available 0000000080000000-0000000087FFFFFF 0000000000008000 000000000000000F
    Available 000000008C000000-00000000FFAB2FFF 0000000000073AB3 000000000000000F
    BS_Data   00000000FFAB3000-00000000FFFFFFFF 000000000000054D 000000000000000F
    Available 0000000880000000-00000008FACC1FFF 000000000007ACC2 000000000000000F
    LoaderCode 00000008FACC2000-00000008FADB9FFF 00000000000000F8 000000000000000F
    BS_Code   00000008FADBA000-00000008FAFA9FFF 00000000000001F0 000000000000000F
    Available 00000008FAFAA000-00000008FC5F6FFF 000000000000164D 000000000000000F
    BS_Data   00000008FC5F7000-00000008FC707FFF 0000000000000111 000000000000000F
    Available 00000008FC708000-00000008FC755FFF 000000000000004E 000000000000000F
    BS_Data   00000008FC756000-00000008FC872FFF 000000000000011D 000000000000000F
    Available 00000008FC873000-00000008FC88FFFF 000000000000001D 000000000000000F
    BS_Data   00000008FC890000-00000008FC8C0FFF 0000000000000031 000000000000000F
    Available 00000008FC8C1000-00000008FC8D8FFF 0000000000000018 000000000000000F
    BS_Data   00000008FC8D9000-00000008FC8F1FFF 0000000000000019 000000000000000F
    Available 00000008FC8F2000-00000008FC92DFFF 000000000000003C 000000000000000F
    BS_Data   00000008FC92E000-00000008FC93BFFF 000000000000000E 000000000000000F
    Available 00000008FC93C000-00000008FC948FFF 000000000000000D 000000000000000F
    BS_Data   00000008FC949000-00000008FC94BFFF 0000000000000003 000000000000000F
    Available 00000008FC94C000-00000008FC96AFFF 000000000000001F 000000000000000F
    BS_Data   00000008FC96B000-00000008FCDEBFFF 0000000000000481 000000000000000F
    Available 00000008FCDEC000-00000008FCE03FFF 0000000000000018 000000000000000F
    BS_Data   00000008FCE04000-00000008FCE25FFF 0000000000000022 000000000000000F
    Available 00000008FCE26000-00000008FCE2CFFF 0000000000000007 000000000000000F
    BS_Data   00000008FCE2D000-00000008FF643FFF 0000000000002817 000000000000000F
    Available 00000008FF644000-00000008FF644FFF 0000000000000001 000000000000000F
    BS_Data   00000008FF645000-00000008FF649FFF 0000000000000005 000000000000000F
    Available 00000008FF64A000-00000008FF64AFFF 0000000000000001 000000000000000F
    BS_Data   00000008FF64B000-00000008FFDDDFFF 0000000000000793 000000000000000F
    Available 00000008FFDDE000-00000008FFE47FFF 000000000000006A 000000000000000F
    BS_Code   00000008FFE48000-00000008FFF6DFFF 0000000000000126 000000000000000F
    RT_Code   00000008FFF6E000-00000008FFFAEFFF 0000000000000041 800000000000000F <-- Runtime
    RT_Data   00000008FFFAF000-00000008FFFFEFFF 0000000000000050 800000000000000F <-- Runtime
    BS_Data   00000008FFFFF000-00000008FFFFFFFF 0000000000000001 000000000000000F
    MMIO      000000000C000000-000000000FFEFFFF 0000000000003FF0 8000000000000001 <-- Runtime
    MMIO      000000001C170000-000000001C170FFF 0000000000000001 8000000000000001 <-- Runtime
      Reserved  :          0 Pages (0)
      LoaderCode:        248 Pages (1,015,808)
      LoaderData:          0 Pages (0)
      BS_Code   :        790 Pages (3,235,840)
      BS_Data   :     15,401 Pages (63,082,496)
      RT_Code   :         65 Pages (266,240) <-- Runtime Code regions
      RT_Data   :         80 Pages (327,680) <-- Runtime Data regions
      ACPI Recl :          0 Pages (0)
      ACPI NVS  :          0 Pages (0)
      MMIO      :     16,369 Pages (67,047,424)
      Available :  1,015,608 Pages (4,159,930,368)
    Total Memory: 4095 MB (4,294,905,856 Bytes)
    Shell>

And when booting Linux:

    Remapping and enabling EFI services.
      EFI remap 0x0008fff6e000 => ffffffc87ff6e000
      EFI remap 0x0008fffaf000 => ffffffc87ffaf000
      EFI remap 0x00000c000000 => ffffff8000080000
      EFI remap 0x00001c170000 => ffffff8000012000
