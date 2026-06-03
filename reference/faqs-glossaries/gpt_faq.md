# GPT FAQ

Frequently asked questins about GPT

### Are there tools to setup and create the EFI GPT partition?

Yes win7 installations set up GPT. Intel EFI technology has ‘partition
utilities’ download: [https://www.intel.com/technology/efi](https://www.intel.com/technology/efi)

### Can an OS install be converted to EFI?

No, this must be on installation with a clean disk.

### In reference to the figure in the UEFI 2.x Section 5.3.1 where does the Logical Block Address (LBA) LBAn exist on a disk?

**LBAn** is the last logical block on a disk and also stores the Backup
GUID Partition Table Header’s fields. See section 5.3 UEFI 2.x for more
details

### In reference to UEFI 2.x section 5.3 GUID Partition Table (GPT) any impact to per due to GPT?

There are advantages for formatting hard drives with GPT:

- 128 Bit Unique Identifier and unique serial number
- Supports many partitions unlike MBR
- Uses a primary and backup table for redundancy.
- Support for greater than 2.2TB hard drives

### When installing a UEFI aware OS when do you create the GPT Partitions?

If there is not a GPT partition then this must done on install. UEFI
needs the GPT partition to create UEFI aware OS

### Why would I need it (UEFI aware OS)?

To get the benefits of UEFI with the OS. One benefit is \>2.2 TB hard
drive support.
