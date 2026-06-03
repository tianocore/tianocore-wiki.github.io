# ArmPkg Binaries

## Prerequisite

    cd $EDK2_ROOT
    . edksetup.sh `pwd`/BaseTools
    make -C BaseTools

## FatPkg (tested revision 71 - 2012-06-08)

    svn co https://svn.code.sf.net/p/edk2-fatdriver2/code FatPkg-svn --username guest
    mv FatPkg-svn/trunk/FatPkg .
    build -a ARM -p FatPkg/FatPkg.dsc -t RVCTLINUX -b RELEASE -y report.log
    build -a AARCH64 -p FatPkg/FatPkg.dsc -t ARMLINUXGCC -b RELEASE -y report.log

## ShellPkg (tested revision 13646 - 2012-08-17)

- Build the Full Shell:

    build -a ARM -p ShellPkg/ShellPkg.dsc -t RVCTLINUX -b RELEASE -y report.log
    build -a AARCH64 -p ShellPkg/ShellPkg.dsc -t ARMLINUXGCC -b RELEASE -y report.log

- Without Shell Profiles:

    build -a ARM -p ShellPkg/ShellPkg.dsc -t RVCTLINUX -b RELEASE  -D NO_SHELL_PROFILES -y report.log
    build -a AARCH64 -p ShellPkg/ShellPkg.dsc -t ARMLINUXGCC -b RELEASE  -D NO_SHELL_PROFILES -y report.log

## EdkShellPkg (tested revision 64 - 2014-01-14)

    cd EdkShellPkg
    svn co https://svn.code.sf.net/p/efi-shell/code/trunk/Shell

- Edit EdkShellPkg.dsc to replace:

    DEFINE EDK_SHELL_DIR             = Shell

By:

    DEFINE EDK_SHELL_DIR             = EdkShellPkg/Shell

- Patch the EdkShell sources:

    cd Shell
    patch -p1 < ../ShellR64.patch

- Build it:

    cd $EDK2_ROOT
    build -a ARM -p EdkShellPkg/EdkShellPkg.dsc -t RVCTLINUX -b RELEASE -y report.log

## EdkShellPkg: Instructions to generate ShellRn.patch

1\. Clone the Shell repository using git git svn clone
[https://svn.code.sf.net/p/efi-shell/code/trunk/Shell](https://svn.code.sf.net/p/efi-shell/code/trunk/Shell)

2\. Apply current patch cd Shell

Ensure you can apply the current patch before to apply it (the parameter
'-p0' might differ): patch -p1 --dry-run \<
\$TIANOCORE_ROOT/EdkShellPkg/ShellR64.patch

Apply the patch: patch -p1 \<
\$TIANOCORE_ROOT/EdkShellPkg/ShellR64.patch

3\. Generate the GIT commit - Ensured all the new files have been added:
git add Library/Aarch64/ Library/Arm/ git commit -m "EDK Shell patch to
support GCC"

4\. Generate the new EDK Shell patch git format-patch -1 --stdout \>
\$TIANOCORE_ROOT/EdkShellPkg/ShellR64.patch
