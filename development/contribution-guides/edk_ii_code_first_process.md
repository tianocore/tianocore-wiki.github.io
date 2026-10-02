# EDK II Code First Process

The EDK II Code First Process is a process by which new features can be added
to UEFI Forum specifications after first having been designed and prototyped
in the open.

This process lets changes and the development of new features happen in the
open, without violating the UEFI forum bylaws which prevent publication of
code for in-draft features/changes.

The process does not in fact change the UEFI bylaws - the change is that the
development (of both specification and code) happens in the open. The resulting
specification update is then submitted to the appropriate working group as an
Engineering Change Request (ECR), and voted on. For the UEFI Forum, this is a
change in workflow, not a change in process.

## UEFI Forum Tracking

ECRs are tracked in a [UEFI Forum Mantis](https://mantis.uefi.org/) instance,
with access restricted to UEFI Forum Members.

## TianoCore Tracking

TianoCore enables this process by using [GitHub issues](https://github.com/features/issues)
to track specification updates, reference implementations, and any other
associated changes, such as links to content within the [TianoCore GitHub](https://github.com/tianocore)
organization, related to the "code first" change.

Code first implementation targeting the EDK II open source project are initially
held in draft pull requests within a TianoCore GitHub repository.

> **Note:** Consider whether the [RFC Process](../../rfc/text/0001-rfc-process.md) is needed.

## Intended workflow

1. Create a new GitHub issue in the primary TianoCore repository for the change using the "Code First" form.
   - Note: The primary repository will most frequently be [edk2](https://github.com/tianocore/edk2).
   - Note: Ensure all specifications impacted by the change are selected in the form.
     - Note: A specification draft change must be included in a markdown file in the "code first dev branch". A file
       must be present for each specification if more than one specification is impacted by the change. Base the file
       content on the template in the Code First GitHub issue submission form.
2. Make the changes in a new branch with the prefix `GI####-<BranchName>` that
   meets the content requirements for the code first process described in this document.
    - Note: `####` in `GI####` is the GitHub issue number from *step 1*.
    - Note: `<BranchName>` is a brief description of the change.
    - Note: Code content must follow the coding style and naming conventions in
      the [Source Code](#source-code) section of this document.
    - Note: Code first pull requests may have PR checks performed to verify that these requirements are met.
3. Push the "code first dev branch" to either:
    1. A fork of the primary repository (e.g. `username/edk2`)
    2. A branch in [edk2-staging](https://github.com/tianocore/edk2-staging).

       Consider this branch a collaboration point for yourself and others that may contribute to the change.
        - If you use a fork of the primary repository, ensure that the fork is public. You may grant permisssions to
          your fork branch as needed for others to collaborate there.
        - If you use an `edk2-staging` branch, you might need to reach out to an edk2-staging maintainer so they can
          grant permissions to the users that need to push changes there.
          - If you do not have write permission, in the "Anything else?" box of the GitHub issue in *step 1*, notify the
            admins with `@tianocore/tianocore-admins` and list the GitHub usernames for all collaborators that need
            permission to the `edk2-staging` branch.
4. Create a draft pull request into the default branch on the repository from the "code first dev branch" (*step 3*).
5. Add a comment in the PR with a link to the GitHub issue created in *step 1*.
   - It is required to link the pull request to the issue following the methods described in
[Linking a pull request to an
issue](https://docs.github.com/en/issues/tracking-your-work-with-issues/using-issues/linking-a-pull-request-to-an-issue).
     - When the issue is linked successfully, it will appear as a link in the "Development" section of the pull request.
       - Note: Establishing this link will automatically add a `type:code-first` label to the pull request. If for some
         reason, this is added in error, or needs to be removed later and you lack sufficient privilege to do so,
         leave a comment in the pull request explaining the situation so a maintainer can address it.
6. Continue to develop the change in the "code first dev branch" until it is ready for review. Changes pushed to the
   branch will automatically update the PR.
7. After all dependent specification changes have been approved and publicly published, the PR with code changes is
   eligible for review. Mark the PR as ready for review when code changes are final (so it is taken out of draft
   status).
8. Reviewers will review the PR and provide feedback.
9. Make changes based on feedback and continue to iterate until the change is ready to be merged.
10. A maintainer will merge the PR after the change is approved.

If the change impacts repsoitories other than edk2, such as integration changes in
[edk2-platforms](https://github.com/tianocore/edk2-platforms), those changes should
be kept in a branch on a fork of the repository and the PR process described above
should be followed in that repository as well.

Any other relevant branches, issues, discussions, or forks should be linked to the issue in *step 1*.

When the change is ready for review, the PR should be marked as ready for review (taken out of draft status).

### Edk2-Staging Branch and the Draft Pull Request

Something to be aware of is that two parts of the code first process are constant to simplify finding and contributing
to code first changes.

1. Regardless of the decision made in *step 3*, a `GI####-<BranchName>` branch will always exist in `edk2-staging`.
2. A draft PR linked to the GitHub issue in *step 1* will always exist in the primary repository.

If the contributor opts to use a fork of the primary repository in *step 3*, an automated process will sync updates to
the `GI####-<BranchName>` branch in `edk2-staging` any time the draft PR is updated. Thus, even if you do not have
write permission to `edk2-staging` your branch will still be created there and kept up to date on your behalf. Be aware
that in this case, the "code first dev branch" is still the branch on the fork as that is the PR branch.

## Source Code

In order to ensure draft code does not accidentally leak into production use,
and to signify when the changeover from draft to final happens, *all* new or
modified lines must have a comment in the following format:

`[CODE_FIRST] <IssueId>`

```admonish important "Comment Requirements"
"BEGIN" and "END" comment markers are not allowed. Each line must have a comment.
```

- `<IssueId>`  is a placeholder for the GitHub Code First issue for the change
  - For example, if the GitHub issue is `10000`, the comment in C source code should be `// [CODE_FIRST] 10000`
- This process does not prescribe how the comment is positioned relative to other code on the line. The comment
  may have any number of surrounding whitespace characters to meet aesthetic preference or code formatting requirements.
- If multiple code first changes affect the same area of code each line should be unique to a given code first change
  with its corresponding comment. If for any reason two code first changes must be on the same line, the comment for each
  code first change should be placed at the end of the line.

---

### Source Code Comment Tagging Examples

#### DEC File

```ini
  ##  @libraryclass  Provides library functions to access SMBUS devices.                    # [CODE_FIRST] 10000
  #                  Libraries of this class must be ported to a specific SMBUS controller. # [CODE_FIRST] 10000
  NewLib|Include/Library/NewLib.h                                                           # [CODE_FIRST] 10000
```

### DSC File

#### PCD Assignment

```ini
[PcdsFixedAtBuild]
  gEfiMdePkgTokenSpaceGuid.NewPcd|0x1    # [CODE_FIRST] 10000
```

#### New Component(s)

```ini
[Components]
  MdePkg/Library/NewLib/NewLib.inf    # [CODE_FIRST] 10000
  MdePkg/Library/NewLib2/NewLib2.inf  # [CODE_FIRST] 10000
```

### C Code

#### GUID Macro

```c
///                                                                                   // [CODE_FIRST] 10000
/// Global ID for EFI_PEI_RECOVERY_BLOCK_IO_PPI                                       // [CODE_FIRST] 10000
///                                                                                   // [CODE_FIRST] 10000
#define EFI_PEI_RECOVERY_BLOCK_IO_PPI_GUID \                                          // [CODE_FIRST] 10000
  { \                                                                                 // [CODE_FIRST] 10000
    0x695d8aa1, 0x42ee, 0x4c46, { 0x80, 0x5c, 0x6e, 0xa6, 0xbc, 0xe7, 0x99, 0xe3 } \  // [CODE_FIRST] 10000
  }                                                                                   // [CODE_FIRST] 10000
```

#### Struct

```c
///                                                                                 // [CODE_FIRST] 10000
/// Specification inconsistency here:                                               // [CODE_FIRST] 10000
/// PEI_BLOCK_IO_MEDIA has been changed to EFI_PEI_BLOCK_IO_MEDIA.                  // [CODE_FIRST] 10000
/// Inconsistency exists in UEFI Platform Initialization Specification 1.2          // [CODE_FIRST] 10000
/// Volume 1: Pre-EFI Initialization Core Interface, where all references to        // [CODE_FIRST] 10000
/// this structure name are with the "EFI_" prefix, except for the definition       // [CODE_FIRST] 10000
/// which is without "EFI_".  So the name of PEI_BLOCK_IO_MEDIA is taken as the     // [CODE_FIRST] 10000
/// exception, and EFI_PEI_BLOCK_IO_MEDIA is used to comply with most of            // [CODE_FIRST] 10000
/// the specification.                                                              // [CODE_FIRST] 10000
///                                                                                 // [CODE_FIRST] 10000
typedef struct {                                                                    // [CODE_FIRST] 10000
  ///                                                                               // [CODE_FIRST] 10000
  /// The type of media device being referenced by DeviceIndex.                     // [CODE_FIRST] 10000
  ///                                                                               // [CODE_FIRST] 10000
  EFI_PEI_BLOCK_DEVICE_TYPE    DeviceType;                                          // [CODE_FIRST] 10000
  ///                                                                               // [CODE_FIRST] 10000
  /// A flag that indicates if media is present. This flag is always set for        // [CODE_FIRST] 10000
  /// nonremovable media devices.                                                   // [CODE_FIRST] 10000
  ///                                                                               // [CODE_FIRST] 10000
  BOOLEAN                      MediaPresent;                                        // [CODE_FIRST] 10000
  ///                                                                               // [CODE_FIRST] 10000
  /// The last logical block that the device supports.                              // [CODE_FIRST] 10000
  ///                                                                               // [CODE_FIRST] 10000
  UINTN                        LastBlock;                                           // [CODE_FIRST] 10000
  ///                                                                               // [CODE_FIRST] 10000
  /// The size of a logical block in bytes.                                         // [CODE_FIRST] 10000
  ///                                                                               // [CODE_FIRST] 10000
  UINTN                        BlockSize;                                           // [CODE_FIRST] 10000
} EFI_PEI_BLOCK_IO_MEDIA;                                                           // [CODE_FIRST] 10000
```

#### Enum

```c
///                                                                                 // [CODE_FIRST] 10000
/// EFI_PEI_BLOCK_DEVICE_TYPE                                                       // [CODE_FIRST] 10000
///                                                                                 // [CODE_FIRST] 10000
typedef enum {                                                                      // [CODE_FIRST] 10000
  LegacyFloppy   = 0,  ///< The recovery device is a floppy.                        // [CODE_FIRST] 10000
  IdeCDROM       = 1,  ///< The recovery device is an IDE CD-ROM                    // [CODE_FIRST] 10000
  IdeLS120       = 2,  ///< The recovery device is an IDE LS-120                    // [CODE_FIRST] 10000
  UsbMassStorage = 3,  ///< The recovery device is a USB Mass Storage device        // [CODE_FIRST] 10000
  SD             = 4,  ///< The recovery device is a Secure Digital device          // [CODE_FIRST] 10000
  EMMC           = 5,  ///< The recovery device is a eMMC device                    // [CODE_FIRST] 10000
  UfsDevice      = 6,  ///< The recovery device is a Universal Flash Storage device // [CODE_FIRST] 10000
  MaxDeviceType                                                                     // [CODE_FIRST] 10000
} EFI_PEI_BLOCK_DEVICE_TYPE;                                                        // [CODE_FIRST] 10000
```

#### Function Prototype

```
/**                                                                                    // [CODE_FIRST] 10000
  Reads the requested number of blocks from the specified block device.                // [CODE_FIRST] 10000

  The function reads the requested number of blocks from the device. All the           // [CODE_FIRST] 10000
  blocks are read, or an error is returned. If there is no media in the device,        // [CODE_FIRST] 10000
  the function returns EFI_NO_MEDIA.                                                   // [CODE_FIRST] 10000

  @param[in]  PeiServices   General-purpose services that are available to             // [CODE_FIRST] 10000
                            every PEIM.                                                // [CODE_FIRST] 10000
  @param[in]  This          Indicates the EFI_PEI_RECOVERY_BLOCK_IO_PPI instance.      // [CODE_FIRST] 10000
  @param[in]  DeviceIndex   Specifies the block device to which the function wants     // [CODE_FIRST] 10000
                            to talk. Because the driver that implements Block I/O      // [CODE_FIRST] 10000
                            PPIs will manage multiple block devices, PPIs that         // [CODE_FIRST] 10000
                            want to talk to a single device must specify the device    // [CODE_FIRST] 10000
                            index that was assigned during the enumeration process.    // [CODE_FIRST] 10000
                            This index is a number from one to NumberBlockDevices.     // [CODE_FIRST] 10000
  @param[in]  StartLBA      The starting logical block address (LBA) to read from      // [CODE_FIRST] 10000
                            on the device                                              // [CODE_FIRST] 10000
  @param[in]  BufferSize    The size of the Buffer in bytes. This number must be       // [CODE_FIRST] 10000
                            a multiple of the intrinsic block size of the device.      // [CODE_FIRST] 10000
  @param[out] Buffer        A pointer to the destination buffer for the data.          // [CODE_FIRST] 10000
                            The caller is responsible for the ownership of the         // [CODE_FIRST] 10000
                            buffer.                                                    // [CODE_FIRST] 10000

  @retval EFI_SUCCESS             The data was read correctly from the device.         // [CODE_FIRST] 10000
  @retval EFI_DEVICE_ERROR        The device reported an error while attempting        // [CODE_FIRST] 10000
                                  to perform the read operation.                       // [CODE_FIRST] 10000
  @retval EFI_INVALID_PARAMETER   The read request contains LBAs that are not          // [CODE_FIRST] 10000
                                  valid, or the buffer is not properly aligned.        // [CODE_FIRST] 10000
  @retval EFI_NO_MEDIA            There is no media in the device.                     // [CODE_FIRST] 10000
  @retval EFI_BAD_BUFFER_SIZE     The BufferSize parameter is not a multiple of        // [CODE_FIRST] 10000
                                  the intrinsic block size of the device.              // [CODE_FIRST] 10000

**/                                                                                    // [CODE_FIRST] 10000
typedef                                                                                // [CODE_FIRST] 10000
EFI_STATUS                                                                             // [CODE_FIRST] 10000
(EFIAPI *EFI_PEI_READ_BLOCKS)(                                                         // [CODE_FIRST] 10000
  IN  EFI_PEI_SERVICES               **PeiServices,                                    // [CODE_FIRST] 10000
  IN  EFI_PEI_RECOVERY_BLOCK_IO_PPI  *This,                                            // [CODE_FIRST] 10000
  IN  UINTN                          DeviceIndex,                                      // [CODE_FIRST] 10000
  IN  EFI_PEI_LBA                    StartLBA,                                         // [CODE_FIRST] 10000
  IN  UINTN                          BufferSize,                                       // [CODE_FIRST] 10000
  OUT VOID                           *Buffer                                           // [CODE_FIRST] 10000
  );                                                                                   // [CODE_FIRST] 10000
```

#### Modification in An Existing Function

```c
  EFI_BOOT_MODE  BootMode;                                  // [CODE_FIRST] 10000

  . . . Other existing  code

  //                                                        // [CODE_FIRST] 10000
  // Get current Boot Mode                                  // [CODE_FIRST] 10000
  //                                                        // [CODE_FIRST] 10000
  BootMode = GetBootModeHob ();                             // [CODE_FIRST] 10000
  DEBUG ((DEBUG_INFO, "Boot Mode:%x\n", BootMode));         // [CODE_FIRST] 10000

  . . . Other existing code
```
