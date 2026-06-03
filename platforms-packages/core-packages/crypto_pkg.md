# Cryptopkg

[UEFI](../../reference/specs-standards/uefi.md) details interfaces
between the OS and platform firmware. Several security features were
introduced (e.g. Authenticated Variable Service, Driver Signing, etc)
starting in UEFI Specification version 2.2 ([https://www.uefi.org](https://www.uefi.org)). These
security features highly depend on cryptography, which is implemented in
EDK II using CryptoPkg.

[https://github.com/tianocore/edk2/tree/master/CryptoPkg](https://github.com/tianocore/edk2/tree/master/CryptoPkg)

[https://github.com/tianocore/edk2/raw/master/CryptoPkg/Library/OpensslLib/OpenSSL-HOWTO.txt](https://github.com/tianocore/edk2/raw/master/CryptoPkg/Library/OpensslLib/OpenSSL-HOWTO.txt)

For [EDK II](../../reference/external-resources/edk_ii.md) branches
prior to [UDK2017](../../releases-history/archives/udk2017.md)
... There is a dependency on a specific OpenSSL version for your
workspace. Please refer to

    <YourWorkSpace>/CryptoPkg/Library/OpensslLib/Patch-HOWTO.txt

for information on how to download and apply patches required to compile
CryptoPkg.
