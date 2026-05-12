# Unix Like Systems

`Note: New build instructions are available. It is recommended to start with the new instructions if learning how to`
`build edk2 for the first time. This page is retained for reference.`

New instructions: [Build Instructions](../build-workflows/build_instructions.md)

This page provides *step-by-step instructions* for setting up a [EDK
II](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II/) build environment on various
Unix-like systems.

```admonish note "Recommended Alternative"
These instructions are not recommended for most EDK II
developers. If you are working with a supported Linux distribution, then
the [Using EDK II with Native GCC](using_edk_ii_with_native_gcc.md)
instructions are faster, easier, and produce a smaller UEFI binary
image.
```

## Getting started for UNIX-like operating systems

These instructions will be written as a series of commands executed from
a command terminal.

Often these instructions will contain a command which needs to be
executed in the terminal window. For example:

    bash$ echo this bold text is a sample command

To execute this command, highlight the bold text of the command in your
web browser. Most web browsers should be able to copy the text by
selecting Copy under the Edit menu. Now, change back to the terminal
application, and there should be a Paste operation under the Edit menu.
After pasting the command into the shell, you may need to press the
enter or return key to execute the command.

Of course, there may be other ways to copy and paste the command into
the terminal which are specific to the windowing environment and
applications that you are using. If all else fails, however, you can
type the command by hand.

Some commands are very long, and we use the backslash character (\\ to
tell the shell program that the command is not finished. For example:

    bash$ echo this bold text is a sample command \
            which is broken into two lines

When you copy and paste, make sure you include all lines of the command
(including the backslash (\\ characters). If you are typing the command,
you can remove the backslash character (\\ and combine the lines into a
single line if you prefer.

If a command starts with the sudo command, then you may be prompted for
your user password. This will be the same password as you used to login
to the system.

For the purposes of this set of instructions, we will be using the
following paths.

|  |  |
|----|----|
| Edk2 source tree: | ~/src/edk2 |
| GCC X64 cross-compiler installation: | ~/programs/gcc/x64 |
| GCC IA32 cross-compiler installation: | ~/programs/gcc/ia32 |
| Intel ASL Compiler installation: | ~/src/acpica-unix-20090521/compiler/iasl (symlink to compiler at ~/programs/iasl) |

You will need to change the commands if you want to use different
locations, but this is not recommended unless you are sure that you know
what you are doing.

### Internet proxies

If your network utilizes a firewall with a web proxy, then you may need
to configure your proxy information for various command line
applications to work. You may need to consult with your network
administrator to find out the computer name and port to use for proxy
setup. The following commands are common examples of how you would
configure your proxy by setting an environment variable:

    bash$ export http_proxy=https://proxy.domain.com:proxy_port
    bash$ export ftp_proxy=$http_proxy

To utilize the subversion source control command behind an internet
firewall with a web proxy, you should configure the
~/.subversion/servers file.

## Cygwin 1.7.1

Notes:

- Cygwin is not officially supported or tested by the edk2 project at
  this time.
- Building of [EDK II](https://github.com/tianocore/tianocore.github.io/wiki/EDK-II/) components will
  be done using bash.

Prerequisites:

- Cygwin 1.7.1 is currently installed on the system.
- Cygwin 1.7.1 setup.exe is available on the system to install
  additional packages as needed.

Run Cygwin setup.exe and install the following additional packages.

- bison
- flex
- libgmp-devel
- libiconv
- libmpfr-devel
- gcc (use the compiler upgrade helper option under Devel)
- make
- python
- subversion
- wget
- libuuid-devel
- util-linux (for uuidgen)

When pulling source files using subversion you may choose to get them to
a more Windows friendly location. All system drives are mapped under
/cygdrive when using CYGWIN and bash (ie. C:\\ maps to /cygdrive/c).
This allows you to substitute ~/src/edk2 for an alternate path such as
/cygdrive/c/src/edk2 so files can be access at C:\src\edk2 as well.

 Continue with common instructions for Unix

The [remaining instructions](../build-workflows/common_instructions_for_unix.md) are common for
most UNIX-like systems.

## Fedora 11

```admonish note "Platform Status"
- Fedora 11 is not officially supported or tested by the edk2 project at
  this time.
- These instructions assume that the Software Development option was
  enabled during the Fedora installation.
```

### Open the GNOME Terminal program

These instructions will utilize Fedora's built in command shell (bash)
via the GNOME Terminal application. To open the Terminal application,
locate it under the Applications menu and the System Tools sub-menu.

### Install MPFR library

The MPFR library must be installed to allow the gcc cross compiler to be
built.

    bash$ mkdir ~/src
    bash$ cd ~/src
    bash$ wget https://www.mpfr.org/mpfr-current/mpfr-2.4.1.tar.bz2
    bash$ tar -jxf mpfr-2.4.1.tar.bz2
    bash$ cd mpfr-2.4.1
    bash$ ./configure --prefix=/usr
    bash$ make

To install the library, you must be root. Therefore we use 'su' to
become the root user, for the 'make install' command.

    bash$ su
    bash$ make install
    bash$ exit

 Continue with common instructions for Unix

The [remaining instructions](../build-workflows/common_instructions_for_unix.md) are common for
most UNIX-like systems.

## Mac OS X 10.5

### Xcode Tools

The first step is to install the Apple Xcode development environment:

``[`https://developer.apple.com/tools/xcode`](https://developer.apple.com/tools/xcode)

To install Xcode, you must register as an Apple developer, and download
the Xcode installation disk image (which is fairly large). These
instructions were verified with Xcode 3.0. Within the Xcode Tools disk
image, only the Xcode Tools.mpkg package needs to be installed.

### Open OS X Terminal program

Past this point, the remaining instructions will utilize OS X's built in
command shell (bash) via the Terminal application. To open the command
terminal application, open Finder, then press the Cmd-Shift-U key
combination. (This opens the Applications =\> Utilities folder.) In the
Utilities folder, you should see the Terminal application.

### GMP & MPFR (if behind a web proxy)

The gmp and mpfr libraries are needed to build the gcc cross compiler at
a later point in these instructions. Building these libraries on OS X
can present some difficulties, so if you are not behind a network
firewall, then consider using the macports project to install these
libraries. (see below) Be sure to set the http_proxy and ftp_proxy
environment variables before using the 'curl' commands below.

    bash$ mkdir ~/src
    bash$ cd ~/src
    bash$ curl --remote-name \
      ftp://ftp.gnu.org/gnu/gmp/gmp-4.2.2.tar.bz2
    bash$ tar jxvf gmp-4.2.2.tar.bz2
    bash$ cd gmp-4.2.2
    bash$ ./configure --prefix=/usr
    bash$ make
    bash$ make check
    bash$ sudo make install

Note: This might be needed for 64-bit machines if the MPFR configure
fails below.

    bash$ export CFLAGS="-m64"

    bash$ cd ~/src
    bash$ curl --remote-name \
      https://www.mpfr.org/mpfr-current/mpfr-2.4.1.tar.bz2
    bash$ tar -jxf mpfr-2.4.1.tar.bz2
    bash$ cd mpfr-2.4.1
    bash$ ./configure --prefix=/usr
    bash$ make
    bash$ sudo make install

### GMP & MPFR via Macports (if not behind web proxy)

If you are not behind a network firewall, then the
[https://www.macports.org](https://www.macports.org) project can greatly simlify the installation
of gmp & mpfr. (Macports does not work easily with web proxies at this
time.) After installing macports you should be able to simply run this
command at the shell prompt.

    bash$ sudo port install gmp mpfr

 Continue with common instructions for Unix

The [remaining instructions](../build-workflows/common_instructions_for_unix.md) are common for
most UNIX-like systems.

## Ubuntu 9.10

Note: The Ubuntu platform is not officially supported or tested by the
edk2 project at this time.

### Open the GNOME Terminal program

These instructions will utilize Ubuntu's built in command shell (bash)
via the GNOME Terminal application. To open the Terminal application,
locate it under the Applications menu and the Accessories sub-menu.

### Install required software from apt

Several ubuntu packages will be needed to fully set up an edk2 build
environment. In order to easily install all the requirements, you need
to run this command.

    bash$ sudo apt-get install build-essential uuid-dev texinfo \
            bison flex libgmp3-dev libmpfr-dev subversion

## Continue with common instructions for Unix

The [remaining instructions](../build-workflows/common_instructions_for_unix.md) are common for
most UNIX-like systems.
