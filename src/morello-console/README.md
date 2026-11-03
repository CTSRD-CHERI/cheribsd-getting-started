# Accessing the Morello console

You can manage the Morello board, including reaching its multiple serial
consoles and firmware flash storage, via a USB cable connected to a second
computer -- typically a workstation or remote management server.
When you plug the USB cable into the USB-B port (labled **DBG USB**) of
your workstation, a number of different devices will attach (depending on
device drivers on your workstation and Morello firmware configuration),
including:

* Arm Morello USB debug and trace
* Flash storage used for firmware (an on-board SD card)
* Two 4-port USB-to-serial converters

The first three ports of one of the USB-to-serial converters (usually the first)
will be the:

* The MCC management microcontroller console
* The PCC management microcontroller console
* The Morello system console

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

## Connecting from Ubuntu

Linux uses similar device names to FreeBSD.  Typical command lines to
access the MCC and Morello consoles are:

- Management console: `sudo picocom -b 115200 /dev/ttyUSB0`
- Morello console: `sudo picocom -b 115200 /dev/ttyUSB2`

## Connecting from Apple macOS

Each converter has a unique serial number ending in the letter "A" or B".

macOS creates `/dev/cu.usbserial-` device nodes that embed the device
serial number (including `A` or `B`) and port number.
Typical command lines to access the MCC and Morello consoles look like:

- MCC console: `cu -s 115200 -l /dev/cu.usbserial-00FT41683097B0`
- Morello console: `cu -s 115200 -l /dev/cu.usbserial-00FT41683097B2`

You will need to substitute the serial numbers of the USB converters in your
specific Morello board for these commands to work.
Depending on your configuration, the desired consoles may be attached to the
"A" or "B" converter.
See the section below on [Identifying your console](#identifying-your-console)
to see which your system is using.

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

## Identifying your console

Determining which console you have attached to can sometimes be confusing.
Below we show representative output of the more relevant consoles to help.

### MCC console
The MCC console usually sits at a prompt like
```
Cmd>
```
On a soft-reboot the output looks like:
```console
{{#include mcc-reboot.txt}}
```

### PCC console
The PCC console usually sits at a prompt like:
```
pcc_cli>
```
On soft-reboot the output looks like:
```
{{#include pcc-reboot.txt}}
```

Note: the colored prompt on the PCC console breaks the "<new-line>~."
pattern of exiting `cu` on MacOS and you may have to kill your
session should you connect.

### SCP console
You should not need to connect to the SCP console, but if USB device
probing causes the converters to be probed in an unexpected order,
you may see this reboot output instead of the expected connection
to the MCC console.

```
{{#include scp-reboot.txt}}}
```
