# Start Using UEFI

[UEFI](../../reference/specs-standards/uefi.md) usage and support
is rapidly growing in computing today. Our goal is to enable product
manufacturers to create products that are shipped both supporting and
using UEFI to boot the operating system. If you are interested in the
EDK II implementation of UEFI please see
[Getting Started](getting_started.md) with EDK II.

However, if you are a technology enthusiast, then you may want to try
out UEFI yourself. You should note that UEFI is focused on improving the
firmware to OS interface from a code developer standpoint, and you may
not notice much difference as a user once the operating system has
started.

## Warning

This page discusses methods to potentially install a UEFI compatible
operating system on your computer. This can potentially lead to the loss
of the data on your computer's hard drive. Please only proceed if you do
not have valuable data on your computer's hard drive.

## Tech Support Note

Our community is generally developer focused. We don't provide general
technical support for usage of UEFI products. If you need official
support for your UEFI product, then you should seek technical support
from the product manufacturer.

## UEFI compatible firmware

The first thing you will need in order to use UEFI is a compatible
firmware.

## UEFI compatible motherboard

You may have a UEFI compatible firmware in your motherboard. If your
motherboard listed UEFI compatibility as a product feature, then this is
good. If not, then you might still check the firmware (BIOS) setup, but
your results may vary.

There are many possible ways that the motherboard product may enable
UEFI support. Unfortunately, we can't document them all in this page.
But, you might want to enter the firmware (BIOS) setup program for your
product, and attempt to find a UEFI or EFI enabling question. Sometimes
this can be found under the 'boot' menu of the firmware setup.

## OVMF with QEMU or KVM

The [OVMF](https://www.tianocore.org/ovmf/) project is part of our
community tasks, and provides a UEFI compatible firmware for the
[QEMU](https://en.wikipedia.org/wiki/QEMU) and
[KVM](https://en.wikipedia.org/wiki/Kernel-based_Virtual_Machine)
virtual machine environments.

## VirtualBox

VirtualBox ([https://www.virtualbox.org](https://www.virtualbox.org)) is a virtual machine environment.
The more recent versions of VirtualBox can enable UEFI support in the VM
settings under 'System' =\> 'Extended Features' =\> 'EFI support'.

## UEFI compatible operating systems

Our community does not produce a UEFI compatible operating system.
However, here we will list some operating systems which have some level
of UEFI compatibility.

## Ubuntu

[Ubuntu](uefi_ubuntu.md)
provides UEFI bootable CD images for their 64-bit (X64) releases.
