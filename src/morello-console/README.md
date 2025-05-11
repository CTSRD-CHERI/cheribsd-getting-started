# Accessing the Morello console

You can manage the Morello board, including reaching its multiple serial
consoles and firmware flash storage, via a USB cable connected to a second
computer -- typically a workstation or remote management server.
When you plug the USB cable into your workstation, a number of different
devices will attach (depending on device drivers on your workstation),
including:

* Arm Morello USB debug and trace
* Flash storage used for firmware (an on-board SD card)
* Two 4-port USB-to-serial converters

Each controller has a unique serial number ending in the letter "A" or B".
The serial-port converters attach to UARTs associated with:

* The MCC management microcontroller (converter B port 0)
* The PCC management microcontroller (converter B port 1)
* The Morello system console (converter B port 2)

In normal use, only the MCC and Morello system console ports will be used.
You can connect to these using a serial access tool such as cu(1), or a full
terminal emulation environment such as screen(1).
The precise names of the `/dev` device nodes appearing on the workstation
depend on the operating system being used.
Often access will also require running the terminal command under su(8) or
sudo(8), or adding your user to a `dialer` or similar group to grant access to
the USB device nodes.

## Connecting from FreeBSD

FreeBSD names USB device nodes in the order they probe and attach.
Typical command lines to access the MCC and Morello consoles are:

- MCC console: `cu -s 115200 -l /dev/cuaU0`
- Morello console: `cu -s 115200 -l /dev/cuaU2`

## Connecting from Apple macOS

macOS creates `/dev/cu.usbserial-` device nodes that embed the device
serial number (including `A` or `B`) and port number.
Typical command lines to access the MCC and Morello consoles are:

- MCC console: `cu -s 115200 -l /dev/cu.usbserial-00FT41683097B0`
- Morello console: `cu -s 115200 -l /dev/cu.usbserial-00FT41683097B2`

You will need to substitute the serial numbers of the USB converters in your
specific Morello board for these commands to work.

## Connecting from Windows

Windows requires the installation of FTDI drivers to access the Morello
board's USB ports, and the OS should identify the appropriate driver version
once the board is connected.
Plink is a CLI bundled with PuTTY, a third-party terminal emulator and SSH
client freely available from the 
[Windows Store](https://apps.microsoft.com/detail/putty/XPFNZKSKLBP7RJ)
and the developer's official
[website](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html).
After installing the drivers and PuTTY, typical command lines to access the
MCC and Morello consoles are:

- MCC console: `plink -serial \\.\COM3 -sercfg 115200`
- Morello console: `plink -serial \\.\COM5 -sercfg 115200`

Do note that Windows assigns serial numbers at random.
