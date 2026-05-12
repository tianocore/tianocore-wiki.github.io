# SecurityPkg

There are 4 security related features in SecurityPkg including
[TPM](security_pkg.md#tpm),
[User identification (UID)](security_pkg.md#uid),
[secure boot](security_pkg.md#secure-boot), and
[authenticated variable](security_pkg.md#authenticated-variable).

More information:

- [ReadMe_Security.txt](https://github.com/tianocore-docs/Docs/blob/main/UDK/UDK2010.SR1/Readme_Security.txt)
- [How to enable Security](../../development/tutorials-howto/how_to_enable_security.md) with EDK
  II
- PDF on [How to Sign UEFI Drivers & Applications
  V1.31.pdf](https://github.com/tianocore-docs/Docs/blob/main/User_Docs/SigningUefiImages%20-v1dot31.pdf)
- [Secure Boot Ecosystem Challenges
  PDF](https://docs.google.com/file/d/0BxgB4JDywk3MWnM0WmNXMHBTcm8/edit?pli=1)-From
  ToorCamp 2012, Presentation by Vincent Zimmer
- White Paper : [A Tour Beyond BIOS into UEFI Secure Boot
  PDF](https://github.com/tianocore-docs/Docs/blob/main/White_Papers/A_Tour_Beyond_BIOS_into_UEFI_Secure_Boot_White_Paper.pdf)

## TPM

The Trusted Platform Module (TPM) is a passive hardware device on the
system board. UEFI TCG Measured Boot takes use of TPM device to
construct the basic trusted computing environment based on S-CRTM
(Static Core Root of Trust for Measurement). The implementation includes
a set of TCG-compliant modules that provide the necessary functionality
as described by the TCG specifications for the TPM component. These
modules deeply rely on the TCG-defined specifications. Basically, they
provide the capabilities to initialize and operate a TPM device and
implement the TCG-defined services (performing measurement and logging
data/events, physical presence, memory overwrite request, etc) for
S-CRTM based attestation of the UEFI platform.

## UID

The UID framework implements EFI_USER_MANAGER_PROTOCOL,
EFI_USER_CREDENTIAL_PROTOCOL and EFI_DEFERRED_IMAGE_LOAD_PROTOCOL, and
supports user identity management, user access privilege management,
user identification policy management, static multi-factor
authentication, and publishes the current user profile in the EFI System
Configuration Table after user authentication.

## Secure Boot

This secure boot mechanism allows the firmware to authenticate a UEFI
image, such as an operating system loader or an option ROM. This
provides the capability to ensure that the firmware drivers are loaded
only in an owner-authorized fashion and provides a common means to
ensure platforms security and integrity over systems running UEFI-based
firmware.

## Authenticated Variable

Authenticated variable service is an enhancement on Variable Service in
UEFI, which provides a means to restrict operation on certain sensitive
storage data. If the EFI_VARIABLE_AUTHENTICATED_WRITE_ACCESS is set,
both the identity of operator and integrity of data will be
authenticated before write the variable into storage.

## Signing Tools

This section lists signing tools and resources for UEFI Secure Boot on
Linux. It is far from an exhaustive list but hopefully will be a helpful
starting point. This version was created Jan. 31, 2013 and will be
updated periodically. Please send recommended changes to the
[edk2-devel mailing list](mailto:edk2-devel@lists.01.org?subject=Boot%20Signing%20Tools%20Question).
To join the email list goto:
[edk2-devel](../../community/communications/mailing_lists.md)
(NOTE: Do NOT send any personal information to the edk2-devel list.)

### Linux tools for signing UEFI images

<table  width="100%" style="border-collapse: collapse" bordercolor="#0000011">

<tr>

<td>

**Tool**

</td>

<td>

**Link**

</td>

</tr>

<tr>

<td width="141">

 sbsigntool

</td>

<td>

<git://kernel.ubuntu.com/jk/sbsigntool>

``

</td>

</tr>

<tr>

<td width="141">

pesign

</td>

<td>

``

[https://github.com/vathpela/pesign](https://github.com/vathpela/pesign)

</td>

</tr>

</table>

### Linux Secure Boot Resources

- - UEFI Secure Boot support in several Linux distros,
  - creating keys and certificates,
  - signing images for development and production,
  - And more …

------------------------------------------------------------------------

<table width="100%">

<tr>

<th>

Link

</th>

<th>

Description

</th>

</tr>

<tr>

<th colspan="2" style="background-color:#a9c6dd">

**Ubuntu**

</th>

</tr>

<tr>

<td>

[ubuntu](https://wiki.ubuntu.com/SecurityTeam/SecureBoot)

</td>

<td width=150>

OVMF tips, creating keys & certificates, configuring secure boot ,
signing test images with sbsign, updating key databases

</td>

</tr>

<tr>

<td>

[jk.ozlabs.org](https://jk.ozlabs.org/docs/sbkeysync-maintaing-uefi-key-databases/)

</td>

<td>

creating keys & certificates, tools for signing variables and
maintaining key databases

</td>

</tr>

<tr>

<th colspan="2" style="background-color:#a9c6dd">

**SUSE**

</th>

</tr>

<tr>

<td>

[overview](https://www.suse.com/blogs/uefi-secure-boot-overview)

</td>

<td>

Backgrounder

</td>

</tr>

<tr>

<td>

[suse.com](https://www.suse.com/blogs/uefi-secure-boot-pla)

</td>

<td>

shim loader description

</td>

</tr>

<tr>

<td>

[suse.com/details](https://www.suse.com/blogs/uefi-secure-boot-details)

</td>

<td>

MOK description

</td>

</tr>

<tr>

<td>

[en.opensuse.org](https://en.opensuse.org/openSUSE:UEFI_Secure_boot_using_qemu-kvm)

</td>

<td>

Secure Boot in OVMF & lockdown.efi

</td>

</tr>

<tr>

<td>

[en.opensuse.org](https://en.opensuse.org/SDB:UEFI_EDK2_Build_Howto_On_openSUSE12_1)

</td>

<td>

Building EDK2

</td>

</tr>

<tr>

<th colspan="2" style="background-color:#a9c6dd">

**Fedora**

</th>

</tr>

<tr>

<td>

[fedoraproject.org](https://fedoraproject.org/wiki/Secureboot)

</td>

<td>

Backgrounder

</td>

</tr>

<tr>

<td>

[Secureboot](https://fedoraproject.org/wiki/Features/SecureBoot)

</td>

<td>

Status

</td>

</tr>

<tr>

<td>

[Testing](https://fedoraproject.org/wiki/Secure_Boot_Testing)

</td>

<td>

Images for testing

</td>

</tr>

<tr>

<th colspan="2" style="background-color:#a9c6dd">

**Linux Foundation**

</th>

</tr>

<tr>

<td>

[uefi-secure-boot](https://blog.hansenpartnership.com/uefi-secure-boot/)

</td>

<td>

using MS UEFI CA to sign images

</td>

</tr>

</table>

## Signing - Secure Boot Results

#### a Linux Loaders and Kernels

- EFI_STUB Kernel - has been signed with the Microsoft\* SignTool and
  successfully secure booted. Fixes are available in <tip:x86/efi>
  branch at:

[https://git.kernel.org/?p=linux/kernel/git/tip/tip.git;a=shortlog;h=refs/heads/x86/efi](https://git.kernel.org/?p=linux/kernel/git/tip/tip.git;a=shortlog;h=refs/heads/x86/efi)

- efilinux Loader - has been signed with the Microsoft\* SignTool and
  successfully secure booted. These fixes have been merged into the
  'next' branch prior to being merged into the 'master' branch and a
  planned 1.1 release:

[https://github.com/mfleming/efilinux](https://github.com/mfleming/efilinux)

The .reloc section fix on x86-64 was merged into the efi-linux library’s
sourceforge repository and a new version was released (3.0q).

#### UEFI Applications Built with UDK Linux Tool Chains

- UDK GCC44 & GCC46 UDK Tool Chain

UEFI applications (such as HelloWorld.efi) built with the GCC44 and
GCC46 UDK tool chains have been signed with the Microsoft\* SignTool and
successfully secure booted.

- UDK UNIXGCC UDK Tool Chain

UEFI applications built with the UNIXGCC tool chain are not currently
secure bootable. This problem is under investigation.

==**Images with Multiple Signatures**== This section describes support
for Authenticode-signed UEFI images with multiple signatures, references
sample certificates and images and describes patches to enable this
support .

### 1. Background

The Authenticode signature is in a location called the "Attribute
Certificate Table" in a UEFI image. According to the UEFI and PE/COFF
specifications, the Attribute Certificate Table is composed of a set of
contiguous certificate entries (a list of certificates), and each
certificate entry contains an independent Authenticode signature.

However, revision 2.3.1B of the UEFI Specification has no explicit
description about the verification policy for the multiple certificate
entries in a UEFI image, but the Authenticode specification does not
preclude this case of multiple entries.

Currently, both the SignTool from MSFT and the DxeImageVerificationLib
from the SecurityPkg on [https://www.tianocore.org](https://www.tianocore.org) treat the Attribute
Certificate Table as a single certificate. So if one re-signs a UEFI
image with a new certificate, the MSFT SignTool will overwrite the old
certificate entry with the new one. And the verification library assumes
that there is only one certificate entry in the attribute certificate
table.

Other Sign Tools are now emerging that can create UEFI images with
multiple signatures.

### 2. Sample Images

Some sample UEFI images and their certificates can be found at:
[https://pjones.fedorapeople.org/multisign/](https://pjones.fedorapeople.org/multisign/)

They are:

- pjones.cer - public key with a DN that basically says "pjones"
- PeterJones.cer - different public key with a DN that says "PeterJones"
  instead
- hello-0.efi - "Normal, unsigned HelloWorld.efi
- hello-1.efi - same binary, signed with "pjones"
- hello-2.efi - same binary, signed with "pjones" and then with
  "PeterJones".

Note: the 28-Jun-2012 12:41 version of both .cer files \[which had
lengths of 294\] were corrupt.

### 3. DxeImageVerificationLib Patch

Verification of UEFI images with multiple signatures can be enabled with
a small patch to the IsPkcsSignedDataVerifiedBySignatureList routine in
SecurityPkg\Library\DxeImageVerificationLib\DxeImageVerificationLib.c.

Patches are based based on SVN r13505 of
[https://svn.code.sf.net/p/edk2/code/trunk/edk2/](https://svn.code.sf.net/p/edk2/code/trunk/edk2/)

Rev2 handles N certificates in an image and validates each
WIN_CERTIFICATE header found in the image

Rev1 only supported 2 certificates in an image.

- Rev2 - patch
  [DxeImageVerificationLib-MultipleCertPOC-rev2-r13505.patch](https://github.com/tianocore-docs/Docs/blob/main/UDK/UDK2010.SR1.UP1/DxeImageVerificationLib-%20MultipleCertPOC-rev2-r13505.patch)
- Rev1 – patch
  [DxeImageVerificationLib-MultipleCertPOC-r13505.patch](https://github.com/tianocore-docs/Docs/blob/main/UDK/UDK2010.SR1.UP1/DxeImageVerificationLib-MultipleCertPOC-r13505.patch)

### 4. Scenarios

- Enroll pjones.cer in PK, KEK & DB,
  - both hello-1.efi and hello-2.efi run.
- Enroll PeterJones.cer in PK, KEK & DB,
  - now only hello-2.efi runs.
