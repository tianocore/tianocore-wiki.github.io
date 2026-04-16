# Using EDK II With Native GCC

``` admonish note "Note: New instructions"
New build instructions are available. It is recommended to start with the new instructions if learning how to
build edk2 for the first time. This page is retained for reference.

New instructions: [Build Instructions](../build-workflows/build_instructions.md)
```

This page provides *step-by-step instructions* for setting up a [EDK
II](http://www.tianocore.org/edk2/) build environment on Linux using a
native GCC installation (4.4+). This EDK II setup does not require the
Mingw version of GCC to be built, and therefore can be much faster to
setup.

## Getting Started

These instructions will be written as a series of commands executed from
a command terminal.Often these instructions will contain a command which
needs to be executed in the terminal window. For example:

```bash
bash$ echo this text is a sample command
```

To execute this command, highlight the text of the command in your web
browser. (Note that the 'bash\$' text is not part of the command!) Most
web browsers should be able to copy the text by selecting Copy under the
Edit menu. Now, change back to the terminal application, and there
should be a Paste operation under the Edit menu. After pasting the
command into the shell, you may need to press the enter or return key to
execute the command.

Of course, there may be other ways to copy and paste the command into
the terminal which are specific to the windowing environment and
applications that you are using. If all else fails, however, you can
type the command by hand.

Some commands are very long, and we use the backslash character (\\) to
tell the shell program that the command is not finished. For example:

```bash
bash$ echo this bold text is a sample command \
        which is broken into two lines
```

When you copy and paste, make sure you include all lines of the command
(including the backslash (\\) characters). If you are typing the command,
you can remove the backslash character (\\) and combine the lines into a
single line if you prefer.

If a command starts with the sudo command, then you may be prompted for
your user password. This will be the same password as you used to login
to the system.

For the purposes of this set of instructions, we will be using the
following paths.

|                                       |                 |
|---------------------------------------|-----------------|
| Edk2 source tree:                     | \$HOME/src/edk2 |
| Native GCC 4.x compiler installation: | /usr/bin/gcc    |
| Intel ASL Compiler installation:      | /usr/bin/iasl   |

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

```bash
bash$ export http_proxy=http://proxy.domain.com:proxy_port
bash$ export ftp_proxy=$http_proxy
```

To utilize the subversion source control command behind an internet
firewall with a web proxy, you should configure the
~/.subversion/servers file.

### Ubuntu 20.04 LTS

Note: These instructions utilize GCC5 & NASM compiler support (added in
early 2016), along with git (replaces subversion). GCC 4.x is still
supported. GCC5 is not mandatory.

#### Open the GNOME Terminal program

These instructions will utilize Ubuntu's built in command shell (bash)
via the GNOME Terminal application. To open the Terminal application,
locate it under the Applications menu and the Accessories sub-menu.

#### Install required software from apt

Several Ubuntu packages will be needed to set up the build environment
for EDK II. The following command will install all required packages:

```bash
bash$ sudo apt install build-essential uuid-dev iasl git  nasm  python-is-python3
```

`build-essential` - Informational list of build-essential packages

`uuid-dev` - Universally Unique ID library (headers and static libraries)

`iasl` - Intel ASL compiler/decompiler (also provided by acpica-tools)

`git` - support for git revision control system

`nasm` - General-purpose x86 assembler

`python-is-python3` - Ubuntu 20.04 python command is 'python3' but edk2
tools use 'python'

### Continue with common instructions

The [remaining instructions](../build-workflows/common_instructions.md) are common for
most UNIX-like systems.

### Ubuntu 16.04 LTS / Ubuntu 16.10

Note: These instructions utilize GCC5 & NASM compiler support (added in
early 2016), along with git (replaces subversion). GCC 4.x is still
supported. GCC5 is not mandatory.

#### Open the GNOME Terminal program

These instructions will utilize Ubuntu's built in command shell (bash)
via the GNOME Terminal application. To open the Terminal application,
locate it under the Applications menu and the Accessories sub-menu.

#### Install required software from apt

Several Ubuntu packages will be needed to set up the build environment
for EDK II. The following command will install all required packages:

```bash
bash$ sudo apt-get install build-essential uuid-dev iasl git gcc-5 nasm python3-distutils
```

`build-essential` - Informational list of build-essential packages

`uuid-dev` - Universally Unique ID library (headers and static libraries)

`iasl` - Intel ASL compiler/decompiler (also provided by acpica-tools)

`git` - support for git revision control system

`gcc-5` - GNU C compiler (v5.4.0 as of Ubuntu 16.04 LTS)

`nasm` - General-purpose x86 assembler

`python3-distutils` - distutils module from the Python standard library

### Continue with common instructions

The [remaining instructions](../build-workflows/common_instructions.md) are common for
most UNIX-like systems.

## Instructions for older Linux Environments

Note: the instructions below have not been updated for compilers newer
than GCC 4.4, git (replaces subversion) or NASM. Newer builds may fail
without satisfying these dependecies. We recommend moving to newer Linux
distributions unless you have a dependency on a specific version.

### Note: Limitations of GCC 4.4

Note: The GCC 4.4 toolchain only supports building images for the IA32
and X64 architectures. We recommend using newer toolchains.

Also, in some cases if GCC 4.4 is installed under Linux x86 (32-bit
mode), then it may only support building UEFI images for the IA32
architecture.

### Arch Linux 2010.05

Note: Arch Linux is not officially supported or tested by the edk2
project at this time.

#### Open the GNOME Terminal program

These instructions will utilize Arch Linux's built in command shell
(bash) via the GNOME Terminal application. To open the Terminal
application, locate it under the Applications menu and the System Tools
sub-menu.

#### Install required software with pacman

To install the required packages, you must be root. Therefore we use
'su' to become the root user.

```bash
bash$ su -
bash$ pacman -S base-devel glibc iasl python2 subversion
bash$ exit
```

Continue with common instructions

The [remaining instructions](../build-workflows/common_instructions.md) are common for
most UNIX-like systems.

### Fedora 13

Note: Fedora is not officially supported or tested by the edk2 project
at this time.

Note: x86 (32-bit) Fedora will install GCC 4.4 which is only capable of
building UEFI for the IA32 architecture.

#### Open the GNOME Terminal program

These instructions will utilize Fedora's built in command shell (bash)
via the GNOME Terminal application. To open the Terminal application,
locate it under the Applications menu and the System Tools sub-menu.

#### Install required software with yum

To install the required packages, you must be root. Therefore we use
'su' to become the root user.

```bash
bash$ su -
bash$ yum groupinstall development-tools
bash$ yum install iasl libuuid-devel
bash$ exit
```

Continue with common instructions

The [remaining instructions](../build-workflows/common_instructions.md) are common for
most UNIX-like systems.

### Mandriva 2010

Note: Mandriva is not officially supported or tested by the edk2 project
at this time.

Note: x86 (32-bit) Mandriva will install GCC 4.4 which is only capable
of building UEFI for the IA32 architecture.

#### Open the Terminal program

These instructions will utilize Mandriva's built in command shell (bash)
via the Terminal application. To open the Terminal application, locate
it under the Applications menu and the Tools sub-menu.

#### Install required software with urpmi

To install the required packages, you must be root. Therefore we use
'su' to become the root user.

```bash
bash$ su -
bash$ urpmi task-c++-devel iasl libuuid-devel subversion
bash$ exit
```

Continue with common instructions

The [remaining instructions](../build-workflows/common_instructions.md) are common for
most UNIX-like systems.

### openSUSE 12.1

Note: openSUSE is not officially supported or tested by the edk2 project
at this time.

#### Open the GNOME Terminal program

These instructions will utilize openSUSE's built in command shell (bash)
via the GNOME Terminal application. To open the Terminal application,
click the 'Computer' menu, click the 'More Applications' button, and
then enter 'terminal' into the filter text box.

#### Install required software with zypper

Several openSUSE packages will be needed to fully set up an edk2 build
environment. In order to easily install all the requirements, you need
to run this command.

```bash
bash$ sudo zypper in -t pattern devel_basis
```

Continue with common instructions

The [remaining instructions](../build-workflows/common_instructions.md) are common for
most UNIX-like systems.

### Ubuntu 10.10

Notes:

- The Ubuntu platform is not officially supported or tested by the edk2
  project at this time.
- Both the x86 (32-bit) and x86-64 (64-bit) Ubuntu versions will install
  GCC 4.4 which is capable of building UEFI for both the IA32 and X64
  architectures.
- These instructions should work for Ubuntu 10.04 as well

#### Open the GNOME Terminal program

These instructions will utilize Ubuntu's built in command shell (bash)
via the GNOME Terminal application. To open the Terminal application,
locate it under the Applications menu and the Accessories sub-menu.

#### Install required software from apt

Several ubuntu packages will be needed to fully set up an edk2 build
environment. In order to easily install all the requirements, you need
to run this command.

```bash
bash$ sudo apt-get install build-essential subversion uuid-dev iasl
```

### Continue with common instructions

The [remaining instructions](../build-workflows/common_instructions.md) are common for
most UNIX-like systems.

## See Also

- [Unix-like systems](unix_like_systems.md) - Instructions which
  walk through building the Mingw GCC cross-compiler
