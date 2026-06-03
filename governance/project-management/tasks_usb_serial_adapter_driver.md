# Tasks USB Serial Adapter Driver

Develop a USB driver for a common USB Serial Adapter.

- Status: Complete :heavy_check_mark:
- Difficulty: Medium
- Language: C
- Mentor: [https://github.com/ErikBjorge](https://github.com/ErikBjorge)
- Suggested by: [https://github.com/jljusten](https://github.com/jljusten)

## Status

- Completed :heavy_check_mark: (For FTDI).
- Work done by [https://github.com/ashedesimone](https://github.com/ashedesimone)
  as a [GSoC2012](../../community/events-outreach/gsoc2012.md)
  Student.
- Available at
  [https://github.com/tianocore/edk2-platforms/tree/master/Drivers/OptionRomPkg/Bus/Usb/FtdiUsbSerialDxe](https://github.com/tianocore/edk2-platforms/tree/master/Drivers/OptionRomPkg/Bus/Usb/FtdiUsbSerialDxe).

## Details

Today there are many inexpensive USB Serial adapters available, and most
systems are built with USB ports available. But at the same time, the
dedicated Serial port is becoming less common to find available in a
system.

A serial port can still be useful for software debugging purposes.
(debug trace messages)

It can also be useful in providing a secondary terminal to the UEFI
system.

This task would involve writing a USB driver which interfaces with a USB
Serial Adapter.

Ideally, this project should enable a driver that will attach to the USB
Serial Adapter and produce the
[SerialIo](https://sourceforge.net/p/edk2/code/HEAD/tree/trunk/edk2/MdePkg/Include/Protocol/SerialIo.h)
protocol to enable the UEFI terminal to become available through the USB
Serial adapter.

The
[MdeModulePkg/Bus/Usb/UsbKbDxe](https://sourceforge.net/p/edk2/code/HEAD/tree/trunk/edk2/MdeModulePkg/Bus/Usb/UsbKbDxe)
(USB Keyboard) driver may provide a useful starting point for your
project.

## Development environment

Building: This project can be completed on any edk2 supported OS or
toolchain.

## Test environment

## OVMF with FTDI emulated device

With QEMU you can use the '-usb -usbdevice serial::vc' parameters to add
an emulated FTDI into the system.

Support for the FTDI device was worked on during
[GSoC2012](../../community/events-outreach/gsoc2012.md). The
latest code is currently available at:

  [https://github.com/AshleyDeSimone/edk2/tree/ftdi-usb-serial](https://github.com/AshleyDeSimone/edk2/tree/ftdi-usb-serial)
- Look for the driver code under OptionRomPkg/Bus/Usb/FtdiUsbSerialDxe

A quick start for building & testing this code on Linux would be:

    OvmfPkg/build.sh
    OvmfPkg/build.sh qemu -usb -usbdevice serial::vc

Once QEMU/OVMF starts, press Control+Alt+5 to see the 'usbserial'
output.

## QEMU/OVMF with real hardware

[OVMF](https://www.tianocore.org/ovmf/) (on Linux) would probably provide
the easiest environment for testing your project with real hardware. You
should be able to build your driver into OVMF and then run OVMF with the
USB adapter being mapped inside the virtual machine environment. If you
utilize OVMF for testing, then documenting the procedure for running
OVMF to attach to the USB device would be a useful 'extra' side-project.

## Real UEFI system

You could also test your driver on another UEFI system by using the EFI
shell 'load' command.

## Sub-goals

Some possible sub-goals for the driver

- Optional: If using OVMF for testing, document the environment and
  commands you use to enable the adapter to show up in the VM
- recognize and attach to the adapter
- configure the serial port settings for the adapter
- send test characters out the adapter which are successfully read on
  the other side of the serial cable
- receive some test characters, and print them out in debug trace
  messages
- connect to SerialIo to enable a full terminal console

## Example Products

There are many USB Serial adapters available. Some examples are listed
here, but no specific adapter is required or recommended for this
project.

- The QEMU virutal machine environment emulates an FTDI based device
  (see more details above!)
  - [https://www.ftdichip.com/Drivers/D2XX.htm](https://www.ftdichip.com/Drivers/D2XX.htm)
- [Keyspan
  USA-19HS](https://www.tripplite.com/en/products/model.cfm?txtSeriesID=849&txtModelID=3914)
- [Belkin USB to Serial
  adapter](https://www.belkin.com/IWCatProductPage.process?Product_Id=281230)
- [CablesToGo USB to DB9 Serial
  Adapter](https://www.cablestogo.com/product.asp?cat_id=7057&sku=26886)
- [IOGEAR USB to Serial RS-232
  Adapter](https://www.iogear.com/product/GUC232A/)

## Further Discussion

This project would be for an edk2 based driver, so please discuss the
project on [https://edk2.groups.io/g/devel](https://edk2.groups.io/g/devel).

## See Also

- [Tasks#USB-to-serial driver](tasks.md)
