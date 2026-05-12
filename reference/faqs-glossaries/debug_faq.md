# Debug Faq

Debug frequently asked questions

### Is there a way to get a list of or debug message of all the drivers that do not get dispatched? What is recommended
for DSC file and PCD configuration (Debug error level flag) to get a debug output list of all the drivers that were not dispatched?

Debug feature added DEBUG_DISPATCH – this shows the operation of the DXE
and PEI dispatcher as it evaluates every dependency expression of every
PEIM and DXE driver. It will check if the GUID is present or not and the
result of the dependency expression as it runs through the dispatcher.
Documented in the Debug Lib

### Can Serial redirection used with Debug mode enabled?

A\) On platforms where there is only one Serial port and if Source level
debug is enabled then that port is used for the source level debug.
(Build command line option of “-D SOURCE_DEBUG_ENABLE”) However, if the
USB is used for the source debug port then it is possible to use the
serial port for output of Debug print messages and console output.

B\) For USB source debug you need a special cable: NET20DC which can be
found at [https://ajaystech.com/net20dc.htm](https://ajaystech.com/net20dc.htm)

===Why do we still get debug print messages output to the serial port
after changing the Conf\target.txt for “TARGET = RELEASE”? Shouldn’t
this be turned off on a release build?=== A) There are different levels
of debug to be considered. In EDK 1 there was a mix of levels of debug,
DEBUG on and off was a combination of source level debug and debug print
messages. EDK II has separated those concepts. The flag in the
target.txt file “TARGET = RELEASE” only turns off the source level debug
in the output files .pdb. The .pdb file points to the source code on
your host machine which is part of the PE/off image and is part of the
final Flash image. This makes it possible to do source level debug. B)
Another debug concept with EDK II is the DEBUG and ASSERT macros that
print messages to the console and serial port. These are controlled
through the EDK II PCDs in the MdePkg:

- PcdDebugPropertyMask (bit mask to determine turning on/off for various
  macro features )
- PcdDebugPrintErrorLevel (bit map for various types of messages)

C\) A third concept for using source level debugging with WinDBG uses
build option: “-D SOURCE_DEBUG_ENABLE” – builds debug agent into your
firmware. Check example in LakeportX64Pkg.dsc “ifdef”

### It seems the way that the SMM Driver gets dispatched in EDK II has been changed. How does the SMM Driver get
dispatched? Is there any debug message to know which driver is loaded?

a\) You need to select the debug lib in SMM core for report status code.
It is SMM specific to go to serial port b) DEBUG_DISPATCH flag enable in
SMM core for SMM dispatcher

### Can we use the same serial port for FW debug sessions and for an OS channel session?

No. You can’t use same serial port for Firmware debug session & os
channel session

### What is the Macro CPU breakpoint?

CPU breakpoint –comes in as code to cause the processor to halt at that
point.

The EDK II does not have a macro to generate a software breakpoint.
Instead, the BaseLib in the MdePkg has a function called
CpuBreakpoint(). When this function is called a SW breakpoint is
generated. On IA32/X64 platforms, this is an INT 3.
