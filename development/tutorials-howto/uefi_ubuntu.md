# UEFI UBUNTU

[UEFI
OS Support](../../reference/specs-standards/uefi.md) \| [Linux Support for
UEFI](../../reference/specs-standards/uefi.md)

## Ubuntu Linux Support for UEFI

## Ubuntu Versions Supporting UEFI

Ubuntu Linux supports x64
[UEFI](../../reference/specs-standards/uefi.md) for 64-bit builds,
starting with 10.10. We recommend using Ubuntu 12.04 LTS or higher with
x64 UEFI.

No IA32 UEFI support is available in Ubuntu, but is supported in
[UEFI
Debian](uefi_ubuntu.md) starting with the Jessie release.

## Support for UEFI Secure Boot

Ubuntu 12.04 LTS or higher supports
[Secure
Boot](../../reference/specs-standards/uefi.md), with an OS shim loader signed by the
[UEFI CA](../../reference/specs-standards/uefi.md). This
means that a system properly configured for UEFI Secure Boot can load
Ubuntu Linux without enabling legacy BIOS support, or disabling Secure
Boot in the firmware setup menus.

## Downloads

Latest release: [https://www.ubuntu.com/download](https://www.ubuntu.com/download)

Latest live daily images (working towards next release):
[https://cdimage.ubuntu.com/daily-live/](https://cdimage.ubuntu.com/daily-live/)

## Reference Info

[https://help.ubuntu.com/community/UEFI](https://help.ubuntu.com/community/UEFI)

[https://wiki.ubuntu.com/Kernel/Testing/UbuntuUEFI](https://wiki.ubuntu.com/Kernel/Testing/UbuntuUEFI)

[https://askubuntu.com/questions/221835/installing-ubuntu-alongside-a-pre-installed-windows-with-uefi](https://askubuntu.com/questions/221835/installing-ubuntu-alongside-a-pre-installed-windows-with-uefi)
