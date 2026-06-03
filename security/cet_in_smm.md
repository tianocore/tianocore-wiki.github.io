# CET in SMM

Return-oriented Programming (ROP), and similarly call/jmp-oriented programming (COP/JOP), have been
the prevalent attack methodology for stealth exploit writers targeting vulnerabilities in programs.

Control-flow Enforcement Technology (CET) provides the following capabilities to defend against ROP/JOP
style control-flow subversion attacks:

1. Shadow Stack – return address protection to defend against Return Oriented Programming.
2. Indirect branch tracking – free branch protection to defend against Jump/Call Oriented Programming.

For detail of CET, please refer to Control-flow Enforcement Technology whitepaper
([https://www.intel.com/content/www/us/en/content-details/785687/complex-shadow-stack-updates-intel-control-flow-enforcement-technology.html](https://www.intel.com/content/www/us/en/content-details/785687/complex-shadow-stack-updates-intel-control-flow-enforcement-technology.html))

## Introduction

EDKII has enabled different technology for security, such as memory level protection [A Tour Beyound BIOS - Memory
Protection in UEFI BIOS](https://tianocore-docs.github.io/ATBB-Memory_Protection_in_UEFI_BIOS/draft/), or buffer
overflow mitigation [A Tour Beyound BIOS - Mitigate Buffer Overflow in
UEFI](https://tianocore-docs.github.io/ATBB-Mitigate_Buffer_Overflow_in_UEFI/draft/).

Now EDKII can use CET to enforce the control-flow as well. The current status is that EDKII enabled ShadowStatck in SMM.

This wiki page focuses on how to enable CET in SMM.

## PCD

The PCD - PcdControlFlowEnforcementPropertyMask defined in
MdePkg.dec([https://github.com/tianocore/edk2/blob/master/MdePkg/MdePkg.dec](https://github.com/tianocore/edk2/blob/master/MdePkg/MdePkg.dec))
to indicates the control flow enforcement enabling state.

```
  ## Indicates the control flow enforcement enabling state.
  #  If enabled, it uses control flow enforcement technology to prevent ROP or JOP.
  #   BIT0 - SMM CET Shadow Stack is enabled.
  #   Other - reserved
  # @Prompt Enable control flow enforcement.
  gEfiMdePkgTokenSpaceGuid.PcdControlFlowEnforcementPropertyMask|0x0|UINT32|0x30001017
```

## How to enable

1. A real platform may configure gEfiMdePkgTokenSpaceGuid.PcdControlFlowEnforcementPropertyMask in platform dsc to
enable CET in SMM. A emulation platform might not be able to enable this PCD, because enabling CET required supervisor
priviledge.

  ```
  [PcdsFixedAtBuild.common]
    gEfiMdePkgTokenSpaceGuid.PcdControlFlowEnforcementPropertyMask|0x1
  ```

```admonish important
If a platform wants to enable CET, it MUST enable memory protection such as ReadOnly Code Region to ReadOnly,
NonExecutable Data Region.
```

2. If a control flow violation is detected, the system will generate an exception.

  ```
  !!!! X64 Exception Type - 15(#CP - Control Protection)  CPU Apic ID - 00000000 !!!!
  ExceptionData - 0000000000000001
  RIP  - 000000007FDD6326, CS  - 0000000000000038, RFLAGS - 0000000000010006
  RAX  - 000000007FE3EB00, RCX - 0000000000000000, RDX - 000000007FD07140
  RBX  - 000000007EC7D018, RSP - 000000007FE3EB28, RBP - 000000007FE3EC79
  RSI  - 000000007FFE2A01, RDI - 000000007AF3C518
  R8   - 0000000000000000, R9  - 000000007FDD7179, R10 - 0000000000000002
  R11  - 000000007FE3E9A0, R12 - 000000007B2614B8, R13 - 0000000000000003
  R14  - 000000007FFFB3B8, R15 - 0000000076726473
  DS   - 0000000000000020, ES  - 0000000000000020, FS  - 0000000000000020
  GS   - 0000000000000020, SS  - 0000000000000020
  CR0  - 0000000080010033, CR2 - 0000000000000000, CR3 - 000000007FE14000
  CR4  - 0000000000800668, CR8 - 0000000000000000
  DR0  - 0000000000000000, DR1 - 0000000000000000, DR2 - 0000000000000000
  DR3  - 0000000000000000, DR6 - 00000000FFFF0FF0, DR7 - 0000000000000400
  GDTR - 000000007FE12000 000000000000004F, LDTR - 0000000000000000
  IDTR - 000000007FE1C000 00000000000001FF,   TR - 0000000000000040
  FXSAVE_STATE - 000000007FE3E780
!!!! Find image based on IP(0x%x)
c:\edk\Build\PlatformPkg\DEBUG_VS2015x86\X64\TestPkg\StackCookieTest\StackCookieTestSmm\DEBUG\StackCookieTestSmm.pdb
(ImageBase=000000007FDCF000, EntryPoint=000000007FDD0310) !!!!
  ```
