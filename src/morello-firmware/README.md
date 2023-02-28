# Upgrading the Morello firmware

Newer Morello firmware versions contain important feature extensions (such as
support for network booting) and also bug fixes (such as persistent variable
support for UEFI, which is relied on for CheriBSD boot).

More detailed information from Arm on Morello firmware builds and
configuration can be found
[here](https://developer.arm.com/documentation/den0132/0100/Flash-the-onboard-SD-card).
The instructions in this guide are an abbreviated and slightly modified set of
instructions better tuned to CheriBSD requirements.

## Setting the system clock

Start by setting the system date and time in the MCC.
Although the Arm documentation recommends setting your local time, we
recommend using UTC with CheriBSD:

```
Cmd> debug
Debug> time
Debug> date
Debug> exit
```

## Downloading prebuilt firmware

Arm provides [prebuilt firmware
images](https://git.morello-project.org/morello/board-firmware) via the
Morello GitLab instance, although you can also build your own.
Start by downloading this directory tree, which can be done by performing a
`git clone`:

```
git clone https://git.morello-project.org/morello/board-firmware.git
```

## Reflashing the firmware

The Morello firmware is stored on a 2.0GiB SD Card on the Morello board within
the case.
The SD Card content can be exposed as a USB mass storage device accessible
over the USB cable from your workstation.
This may require first enabling USB support on the MCC console:

```
Cmd> USB_ON
Enabling debug USB...
```

The SD Card will attach as a USB disk containing a FAT filesystem.
Depending on your workstation operating system, this may be automatically
mounted (e.g., on macOS), or require manual mounting (e.g., on FreeBSD).
Once mounted, delete the existing firmware and configuration, and then copy on
the new version (substituting an appropriate mounted directory for `/mnt`
below):

```
rm -rf /mnt/*
cp -R board-firmware/* /mnt
sync
```

Depending on your workstation OS and configuration, you may need to run as
root, or use sudo(8), to execute these commands.
Some shells may require confirmation before they will complete the `rm`
command.

**When done, you must unmount the mounted filesystem to ensure that all blocks
in your workstation's filesystem buffer cache have been written back, and to
avoid concurrent accesses leading to possible filesystem corruption.
This must be done before issuing the MCC `reboot` command.**

## Rebooting the MCC

Complete the upgrade by rebooting the Morello board via the MCC:

```
Cmd> reboot
Rebooting...
Disabling debug USB..
```

## Recovering the SD Card format

If you find that your SD Card no longer has a correct partition table
(for example, if like the author you have written an installer image
over it by accident) you will need to restore it to the original state
with a FAT16 formated volume named M1SDP. The mechanism for creating
such a filesystem will vary by OS. After you have created the FAT16
filesystem, follow the instruction above to copy the firmware into place
and install it.

### Linux

On Linux the following commands will create and format the FAT16 partition:

```
parted /dev/sdX mktable msdos
parted /dev/sdX mkpart primary fat16 0% 100%
mkfs.msdos -F 16 /dev/sdX1
```

In this command `/dev/sdX` should be replaced by the path to the SD Card
device.

### MacOS

On MacOS the following command will create and format the M1SDP partition:

```
diskutil partitionDisk /dev/disk# 1 MBRFormat "MS-DOS FAT16" "M1SDP" 1000M
```

In this command `/dev/disk#` should be replaced by the path to the SD Card
device.  The size `1000M` is required, but the actual value is ignored and
the entire disk is used.
