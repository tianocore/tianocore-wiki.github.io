# Tasks Network Block Device

Add [Network Block Device](https://nbd.sourceforge.net/) support

- Difficulty: Medium
- Language: C
- Mentor:
- Suggested by: bjjohnson, andreiwarkentin

## Details

## Test environment

Client

- [OVMF](https://www.tianocore.org/ovmf/),
  [EmulatorPkg](../../platforms-packages/platform-ports/emulator_pkg.md), or
  [NT32](../../archives/platforms-packages/nt32_pkg.md).

Server

- Linux
- Windows (for example, try
  [https://www.vanheusden.com/windows/nbdsrvr/](https://www.vanheusden.com/windows/nbdsrvr/))

## Links for more information

  [https://github.com/NetworkBlockDevice/nbd/blob/master/doc/proto.md](https://github.com/NetworkBlockDevice/nbd/blob/master/doc/proto.md)
- [https://nbd.sourceforge.net/](https://nbd.sourceforge.net/)
- [https://wiki.wireshark.org/NBD](https://wiki.wireshark.org/NBD)
- [https://www.vanheusden.com/java/JNbd/](https://www.vanheusden.com/java/JNbd/)
- [https://www.vanheusden.com/windows/nbdsrvr/](https://www.vanheusden.com/windows/nbdsrvr/)
- [https://github.com/yoe/nbd](https://github.com/yoe/nbd)
  [https://code.activestate.com/recipes/577569-nbd-server-in-python/](https://code.activestate.com/recipes/577569-nbd-server-in-python/)

## Development environment

Building: This project can be completed on any edk2 supported OS or
toolchain.

## Sub-goals

Sub-goals:

- Read the publicly available documentation and understand the NBD
  protocol:
  [https://github.com/NetworkBlockDevice/nbd/blob/master/doc/proto.md](https://github.com/NetworkBlockDevice/nbd/blob/master/doc/proto.md)
  - For any missing details, wireshark can be used to observe existing
    behavior
- Setup UEFI environment with working TCP/IP (can 'ping' from Shell)
- Investigate EDK2 TcpIoLib and ecosystem.
  - Some questions to answer: how to get local IP? Study UEFI spec and
    EDK2 code under NetworkPkg.
  - Write an UEFI application that opens a TCP socket, writes and reads
    from it. Connect to a 'netcat' server. Netcat is a simple tool (like
    cat) that can be a TCP server or client (on Linux, alternatives
    exist for Windows).
  - Extend UEFI application to implement the \*basic\* NBD protocol - no
    named exports, no authentication. Be able to connect to a server and
    read back one block of data.
- Understand UEFI driver model, and block protocols:
  EFI_BLOCK_IO_PROTOCOL and EFI_BLOCK_IO2_PROTOCOL.
  - Better understand protocols, interfaces.
  - Understanding driver binding, enumeration.
  - Implement a dummy EFI_BLOCK_IO_PROTOCOL driver that creates a sample
    block device that returns all zeroes for all reads.
- NBD block I/O device.
  - Wire-in NBD support from your network application above into your
    block I/O driver - implement read-only support.
  - \[EXTRA\] Add read/write support.
  - Think about management interface - how do you specify target
    server/port? (assumed you hardcoded it for now).
  - \[EXTRA\] Write simple tool to manage NBD driver.

## Further Discussion

This project would be for an edk2 based driver, so please discuss the
project on [https://edk2.groups.io/g/devel](https://edk2.groups.io/g/devel).

## See Also

- [Tasks#Network Block
  Device (NBD) client](tasks.md)_client)
