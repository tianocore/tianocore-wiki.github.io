# ArmPlatformPkg AArch64

## Introduction

These instructions are to build and run UEFI on the AArch64 Foundation
or Base FVPs (Fixed Virtual Platforms). FVPs are fixed configurations of
ARM Fast Models; they are also known as RTSMs (Real Time System Models).
The Base FVP is an evolution of the VE (Versatile Express) RTSM.
While the AArch64 Foundation FVP is free to download, the Base FVP
requires an ARM license. The Base FVP has additional debugging and
configuration features.

**Requirement:**

- A 32-bit or 64-bit Linux host machine. Support for MS Windows-based
  toolchains has not been added to the EDK2 BaseTools.

## Getting the EDK2 Source with AArch64 support (ARM 64-bit architecture)

1\) Get the requirements

A Universally Unique Id (UUID) header. Needed to build the EDK2
BaseTools. On Ubuntu: sudo apt-get install uuid-dev

2\) Download the sources

    git clone git@github.com:tianocore/edk2.git
    git clone https://github.com/tianocore/edk2-platforms.git

## Build EDK2 Tianocore

1\) Install AArch64 GNU toolchain:

    sudo apt install gcc-aarch64-linux-gnu

2\) Build EDK2:

    cd $(WORKROOT)/edk2

The first time you create your environment, you will need to build the
EDK2 source tree

    . edksetup.sh
    make -C BaseTools

**Note:** You might need to unset some environment variables if you
are working in the same shell and a different EDK2 repository than your
usual EDK2 development environment.
Otherwise some conflicts might exist between both EDK2 development
environments.

    unset ARCH WORKSPACE

- Declare the EDK2 package paths:

    export PACKAGES_PATH=$PWD:$PWD/edk2-platforms

- For the Foundation and Base FVPs (defined by the DSC file
  ArmPlatformPkg?/ArmVExpressPkg/ArmVExpress-RTSM-AEMv8Ax4-foundation.dsc):

    GCC5_AARCH64_PREFIX=aarch64-linux-gnu- build -a AARCH64 -p Platform/ARM/VExpressPkg/ArmVExpress-FVP-AArch64.dsc -t GCC5

Once built, the UEFI Firmware is the following file
Build/ArmVExpress-FVP-AArch64/DEBUG_GCC5/FV/FVP_AARCH64_EFI.fd

**Note 1:** To build the release build, add '-b RELEASE'. Here's an
example with the Foundation FVP:

GCC5_AARCH64_PREFIX=aarch64-linux-gnu- build -a AARCH64 -p Platform/ARM/VExpressPkg/ArmVExpress-FVP-AArch64.dsc -t GCC5
-b RELEASE

## Start Linux from UEFI on the FVPs

### Build AArch64 Linux

1\) Get the AArch64 Linux sources

    git clone git://git.kernel.org/pub/scm/linux/kernel/git/cmarinas/linux-aarch64.git -b soc-armv8-model
    cd linux-aarch64

2\) Build the AArch64 kernel with Virtio support

    make ARCH=arm64 mrproper
    make ARCH=arm64 defconfig

Enable Virtio Disk and Ext4 support in the kernel. The Linaro disk
file-system uses Ext4.

    make ARCH=arm64 menuconfig
    Device Drivers  ---> Virtio drivers  ---> <*> Platform bus driver for memory mapped virtio devices
    Device Drivers  ---> [*] Block devices  --->  <*> Virtio block driver
    File systems  ---> <*> The Extended 4 (ext4) filesystem

Build the kernel.

```
make -j4 ARCH=arm64
CROSS_COMPILE=<path-to-aarch64-gcc>/gcc-linaro-aarch64-linux-gnu-4.8-2013.06_linux/bin/aarch64-linux-gnu-
```

You should get the binaries:

- arch/arm64/boot/Image
- arch/arm64/boot/dts/foundation-v8.dtb
- arch/arm64/boot/dts/rtsm_ve-aemv8a.dtb

### Run Linux from UEFI on the Foundation FVP

1\) Download the Foundation FVP: [http://www.arm.com/fvp](http://www.arm.com/fvp)

Decompress the AArch64 Foundation FVP

    tar xf ~/FM000-KT-00035-r0p8-48rel5.tgz

2\) The current version of the Foundation FVP can only start an ELF
image. To workaround this limitation, we use the
'uefi-aarch64-bootstrap' to start the UEFI image on this model.

To build the 'uefi-aarch64-bootstrap':

```
    pushd ArmPlatformPkg/ArmVExpressPkg/Scripts/uefi-aarch64-bootstrap/
    CROSS_COMPILE=<path-to-aarch64-gcc>/gcc-linaro-aarch64-linux-gnu-4.8-2013.06_linux/bin/aarch64-linux-gnu- make
    popd
```

3\) The Foundation FVP takes an option for an ELF file to be loaded as
well as an option to load a binary data blob into RAM.

Linux kernel (filename = 'Image') and the Device Tree Binary (filename =
'foundation-v8.dtb') are expected to be found in the directory where the
model is started from.

A file system example can be downloaded from Linaro:

    wget https://releases.linaro.org/13.06/openembedded/aarch64/vexpress64-openembedded_minimal-armv8_20130623-376.img.gz
    gunzip vexpress64-openembedded_minimal-armv8_20130623-376.img.gz

The file-system needs some minimal preparation:

    mkdir tmp
    fdisk -lu vexpress64-openembedded_minimal-armv8_20130623-376.img
    sudo mount -o loop,offset=$((106496 * 512)) vexpress64-openembedded_minimal-armv8_20130623-376.img tmp/
    cd tmp
    sudo ln -s S35mountall.sh etc/rcS.d/S03mountall.sh
    sudo sh -c "echo 'devtmpfs /dev devtmpfs mode=0755,nosuid 0 0' >> etc/fstab"
    cd ..
    sudo umount tmp/

The following command line can be used to run UEFI in the following
manner:

$AARCH64_FOUNDATION_MODEL_ROOT/Foundation_v8 --cores=2
--image=ArmPlatformPkg/ArmVExpressPkg/Scripts/uefi-aarch64-bootstrap/uefi-bootstrap-el3-foundation.axf
--nsdata=Build/ArmVExpress-RTSM-AEMv8Ax4-foundation/DEBUG_GCC47/FV/RTSM_VE_FOUNDATIONV8_EFI.fd@0xA0000000
--block-device=<path/to>/vexpress64-openembedded_minimal-armv8_20130623-376.img

**Note:** Do not use a symbolic link to the file-system image. The
model will not be able to read the image file.

### Run Linux from UEFI on the Base FVP

The Linux kernel (filename = 'Image') and the Device Tree Binary
(filename = 'rtsm_ve-aemv8a.dtb') are expected to be found in the
directory where the model is started from.

    export PATH=ARM_BASE_AEMV8_ROOT:$PATH
    export LD_LIBRARY_PATH=ARM_BASE_AEMV8_ROOT:$LD_LIBRARY_PATH
    FVP_VE_AEMv8A -C motherboard.flashloader0.fname=Build/ArmVExpress-RTSM-AEMv8Ax4/DEBUG_GCC47/FV/RTSM_VE_AEMV8_EFI.fd
