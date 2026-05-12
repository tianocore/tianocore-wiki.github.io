# EFI Toolkit

The [EDKII-EADK](../../platforms-packages/core-packages/edkii_eadk.md)
is replacing the EFI Toolkit. It is recommended that you use this EDK II
Project instead of the EFI Toolkit since it is no longer supported.**

---

## ⚠️ Unmaintained project for reference only

The EFI Toolkit is a set of tools that support rapid porting and
development of EFI applications, and promote a uniform pre-boot
environment on 32 and 64 bit based platforms. The components are
released as reference source code. Some components are meant to be stand
alone and run on UEFI/EFI compliant systems and some components will
require running on the EFI shell. The EFI Toolkit is released under the
[BSD License](../../reference/legal-licenses/bsd_license.md), and the Python code is released
under [PSF
License](https://www.python.org/download/releases/2.4.4/license/).

Working with the EFI Toolkit

The EFI Toolkit package enables rapid development of EFI based
applications, protocols, device drivers, EFI shells, and OS loaders on
IA-32, IA-64(IPF) and Intel 64(EM64T or X64) platforms. It provides
libraries and samples for using native EFI services, as well as
portability libraries that make it easy to port or write Unix/POSIX
style programs.

Source repository:
[https://svn.code.sf.net/p/efi-toolkit/code/trunk/efi-toolkit/](https://svn.code.sf.net/p/efi-toolkit/code/trunk/efi-toolkit/)

``admonish caution "Unmaintained Project"
This project is no longer maintained.
``

## EFI Toolkit Releases

| Release | What is it? | Documents |
|---------|-------------|-----------|
| [EFI_Toolkit-2.0.0.1.zip](https://sourceforge.net/projects/efi-toolkit/files/Official%20Releases/EFI_Toolkit_2.0.0.1.zip/download) | Contains sample EFI applications and utilities source code to aid in EFI development. | Readme<br>[EFI Application Toolkit Release Notes](https://sourceforge.net/projects/efi-toolkit/)<br>General documentation<br>Specifications<br>Technical information |

### What's in the package

- **IA-32, IA-64 and Intel 64 Build Environments** - All source and make files needed to build the IA-32, IA-64 and
  Intel 64(EM64T) versions of the Toolkit
- **Common Utilities** - Hexdump(display raw file contents or medis), Ed (line editor), makramdisk (Create ramdisk)
- **Standard C Library** - The standard functions in the C library (libc, libm, libsociket as well as libefi.
- **Compress/Decompress Library** - Support for doing file compression/decompression is supplied through libz
- **Database Access Library** - The FreeBSD database access method library, libdb
- **TTY Library** - A serial port management library libtty.
- **Network Stack** - A complete TCP/IPv4 implementation as an EFI protocol, and the socket library.
- **Network Utilities** - FTP client, CHCP client, ifconfig, route, hostname, and ping.
- **PPP network support** - A complete PPP implementation is included with the network stack as well as a client and
  serial port management protocol.
- **Scripting Interpreter** - The Python interpreter.
- **Full Screen Text Editor** - Simple full screen text editor that can create and edit both Unicode and ASKII text
  files
- **Wide character (Unicode) and local support** - The C library supports the ANSI wide-character functions and
  FreeBSD* local support.
- **Multi-processor Test support** - Provides support for loading, starting, and terminating non-EFI based programs on
  application processors of a multi-processor Itanium-based system.
- **RAM Disk** - A protocol which provides a virtual (RAM) disk.

License information: [BSD](https://www.opensource.org/licenses/bsd-license.php)

Project owner(s): hlhsiung, mwu7

Developer page: [https://sourceforge.net/projects/efi-toolkit/develop](https://sourceforge.net/projects/efi-toolkit/develop)

## Handy Links

The following links are provided for use by the general public:

- [Directory of EFI Toolkit Source Code
  Snapshots](https://sourceforge.net/projects/efi-toolkit/files/)

- [Link to the TianoCore Community
  Projects](https://sourceforge.net/apps/mediawiki/tianocore/index.php?title=Welcome#Projects)

- [Link to the UEFI Specification (on UEFI.org
  Site)](https://www.uefi.org)

- [Archives of the EDK "Dev" Mailing
  List](https://sourceforge.net/apps/mediawiki/tianocore/index.php?title=Mailing_Lists)

- [Query or submit EFI toolkit Project
  Defects](https://sourceforge.net/tracker/?group_id=289014&atid=1223910)

Project Point of Contact Project owner:

- [Hsiung, Harry L](mailto:harry.l.hsiung@intel.com), Intel Corporation,
- [Wu, Ming](mailto:ming.m.wu@intel.com), Intel Corporation
