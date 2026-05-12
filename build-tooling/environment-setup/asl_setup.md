# ACPI Source Language (ASL) Compiler Setup

EDK II uses ACPI to describe platform configuration and power management methods to the operating system. These methods
are described using ASL, which requires an additional compiler.

[ACPI Component Architecture (ACPICA) Downloads](https://acpica.org/downloads)

[ASL 2.0 Introduction and Overview (whitepaper @ acpica.org)](https://acpica.org/sites/acpica/files/ASL2.0Overview.pdf)

## Windows

Download and install the iasl compiler by downloading the [Windows Binary
Tools](https://acpica.org/downloads/binary-tools) release package. Please check to see if your project requires a
specific ACPICA tools version. Place the unzipped content ("iasl.exe") into the directory "C:\ASL" on your local hard
drive (create the folder "C:\ASL" if it does not exist).

## Linux

Install the [iasl](https://packages.ubuntu.com/search?keywords=iasl) or
[acpica-tools](https://packages.ubuntu.com/yakkety/acpica-tools) package, depending on your Linux distribution. Example
for Debian/Ubuntu:

    bash$ sudo apt-get install iasl
