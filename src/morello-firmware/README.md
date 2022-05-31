# Upgrading the Morello firmware

Newer Morello firmware versions contain important feature extensions (such as
support for network booting) and also bug fixes (such as persistent variable
support for UEFI, which is relied on for CheriBSD boot).

More detailed information from Arm on Morello firmware builds and
configuration can be found
[here](https://developer.arm.com/documentation/den0132/0100/Flash-the-onboard-SD-card).
The instructions in this guide are an abbreviated and slightly modified set of
instructions better tuned to CheriBSD requirements.

**WARNING: These instructions apply to Morello boards with an SoC version of
r0p1 or higher, and are not suitable for a small number of pre-production r0p0
boards distributed in January 2022.
If you have an r0p0 board, please contact Arm directly for advice regarding
firmware upgrades for that board.***

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
cp -r board-firmware/* /mnt
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
