# OVMF

OVMF is an [EDK II](../../reference/external-resources/edk_ii.md) based project to enable
[UEFI](../../reference/specs-standards/uefi.md) support for Virtual Machines. OVMF contains sample UEFI firmware for
[QEMU](https://www.qemu-project.org/) and [KVM](https://www.linux-kvm.org/).

License: [BSD](../../reference/legal-licenses/bsd_license.md)

More information: [OVMF FAQ](../../reference/faqs-glossaries/ovmf_faq.md), [How to build
OVMF](../../development/tutorials-howto/how_to_build_ovmf.md), [Boot Overview](ovmf_boot_overview.md), [edk2-devel](../../community/communications/mailing_lists.md)

Source repository:
[https://github.com/tianocore/edk2/tree/master/OvmfPkg](https://github.com/tianocore/edk2/tree/master/OvmfPkg)

Additional Information

* [https://www.linux-kvm.org/page/OVMF](https://www.linux-kvm.org/page/OVMF)
* [https://wiki.xen.org/wiki/OVMF](https://wiki.xen.org/wiki/OVMF)
* Gerd Hoffmann’s OVMF builds: [https://www.kraxel.org/repos/](https://www.kraxel.org/repos/)
  * These images are automatically built and track the latest OVMF code in the EDK II tree.
* Some of these builds include a seabios CSM and can boot non-UEFI “legacy” operating systems. (Note: seabios is GPLv3
licensed.)
* If your OS doesn’t work with RPM repositories, then you can manually download and decompress the RPM files under
jenkins/edk2
