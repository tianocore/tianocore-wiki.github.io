# EDK Apps

**Note that EDK has been deprecated in favor of
[EDK II](../../reference/external-resources/edk_ii.md). This
information is provided for historical purposes**

## Welcome to the EDK Sample Applications Project

One of the inspiring aspects of the EDK is the ability to develop
applications for the UEFI Environment. Since the ability to expand the
functionality of the UEFI environment through applications is virtually
limitless, this project was created to provide the UEFI community with a
forum to create, share, and discuss applications and their development.
Like the EDK these applications are released under the BSD license.
Refer to the EDK section of the UEFI and Framework Open Source FAQs for
a full explanation of how the EDK relates to the UEFI effort.

### Working with the EDK, Shell, and Sample Applications

The EDK is essentially a container for the Framework's Foundation code
and sample drivers (for more information see The EDK Project).

The EFI Shell is a simple, interactive environment that allows UEFI
device drivers to be loaded, UEFI applications to be launched, and
operating systems to be booted (see the EFI Shell Project for more
information on the shell and its operation).

Applications are code modules that can be built with the EDK and run in
the Shell environment. Applications developed in the EDK can use the
UEFI Framework to perform many useful operations, from basic utilities
(like editing, file manipulation, and user interfaces), to platform
specific support (diagnostics, Protocol tests, device and driver
inventory.

Of particular note, the EFI Shell contains many applications embedded in
it operation, in fact, the shell it self is an application. However, the
shell applications provided in the shell are considered the basic level
of support for the shell environment. The applications contained in this
project were developed beyond that scope (although it is possible to add
new applications and an integrated part the shell).

UEFI applications can be used for many is also a development kit for
developing, debugging, and testing UEFI and Framework drivers, UEFI
Option ROMs, and UEFI Applications for use in the Framework environment.
The following bullets provide information on how to download, build and
use the EDK, Shell, and Sample Applications:

Download snapshots of development or official releases of the EDK.
Simply click on the link and choose the latest development or official
version to start the download process.

Download the EDK Getting Started Guide (pdf) for guidance on building
and using the EDK to develop and test drivers.

Download snapshots of development or official releases of the EFI-SHELL.
Simply click on the link and choose the latest development or official
version to start the download process.

Download the EFI Shell "EDK_Snapshot.txt"

Download the EFI Shell Getting Started Guide (pdf) for guidance on
building and using the EFI-SHELL to develop and test drivers.

Find a sample application you are interested in experimenting with from
the Sample Application Repository, and download the package (each zipped
package contains source and instructions of how to integrate that source
into the EDK with Shell).

In time, you may want to develop applications of your own. If you do,
please consider becoming a contributor to this project and sharing your
applications with the rest of the community. You can find the Framework
Specification on the Intel website.

One last note of applications: While all applications can be built in
the EDK as part of the NT32 build process, some applications will not
execute under NT32. NT32 is an emulation environment, which runs on top
of an operating system. Since advanced operating systems ‘protect’ the
hardware from direct access by user code, Applications designed to
manipulate hardware will not work under NT32. However, those
applications can execute in the DUET environment (for more information
on DUET, see the Add-in Hardware Developers Getting Started
instructions)

**Project Participation** The Sample Application Project originated with
sample applications comprised from efforts done by Intel platform
engineers. However, this project is very open for external Engineers to
contribute new applications, utilities, sample code using the EFI and
UEFI interface. To better understand the different roles of
contribution, please see the Contributor’s Getting Started Guide. (Also,
see the EDK Development Process for an explanation of how this project
functions and how new users can participate.

**Handy Link** The following link is provided for use by the general
public:

- [Directory of Application Source Code
  available](https://sourceforge.net/projects/edk-apps/files/)

Project Points of Contact

- EDK Sample Application Project Owner (US): Michael Krau, Intel
  Corporation, <Michael.p.krau@intel.com>
- EDK Senior Engineer (China): Penny Gao, Intel Corporation,
  <penny.gao@intel.com>
