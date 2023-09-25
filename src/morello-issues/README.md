# Morello known issues

The following known issues may affect the reliability and usability of
CheriBSD on Morello:

- If a USB hub is plugged in (including embedded hubs in USB keyboards) the
  system will pause on boot due to a firmware bug.  Some people find they
  can bypass this pause by pressing Enter on a USB keyboard attached to
  the Morello desktop system.  Others find they must unplug the hub to boot.

- Sometimes the ATA controller starts timing out. and displaying messages
  like this on the console:
  ```
  ahcich0: Timeout on slot 12 port 0
  ahcich0: is 00000000 cs 00003000 ss 00000000 rs 00003000 tfd 50 serr 00000000 cmd 00718c17
  (ada0:ahcich0:0:0:0): WRITE_DMA. ACB: ca 00 50 8a 8b 41 00 00 00 00 d0 00
  (ada0:ahcich0:0:0:0): CAM status: Command timeout
  (ada0:ahcich0:0:0:0): Retrying command, 3 more tries remain
  ```
  A reboot usually cures the problem. If this happens during install,
  it's generally best to start over as performance is awful once timeouts
  start.

- The HDMI transmitter on the board might fail to detect if a monitor is
  plugged in.
  In such a case, the monitor will turn blank when the CheriBSD kernel starts
  printing messages to the console, after the CheriBSD boot loader menu.
  A user can configure the kernel to ignore hot plug detection and assume
  that the attached monitor can receive HDMI input.
  To do that, add the following line to `/boot/loader.conf` and reboot your
  board:
  ```
  hw.tda19988.broken_hpd="1"
  ```
  While the screen itself should be working with this workaround, the user can
  still experience EDID reading failures described below.

- The HDMI transmitter on the board might fail to detect the monitor's
  resolution.
  The CheriBSD kernel writes the following message to the console in such a
  case:
  ```
  tda0: Failed to read EDID
  ```
  Currently, there is no workaround for this issue and it is likely to occur
  with the HDMI transmitter issue described above.

- The ZFS port is unstable with pure-capability kernels. While it does
  support creation of file systems it hangs while mounting a ZFS root file
  system either as a module or compiled in to the kernel. In general it is
  lightly tested and should be used with caution.

- We have witnessed one apparent deadlock in ZFS during an install.
  Dozens of other installs have completed without issue so this is not
  common.

## Issues with older firmware

The following issues affected users of older firmware revisions:

- Due to a firmware bug, OS-triggered reboot may lead the system to hang when
  using older firmware.
  Update your firmware or use the "reboot" command on the management console to
  reboot the system once the OS has shut down.

- Due to a firmware bug, some earlier firmware versions are not able to
  persist UEFI configuration variables.
  This affects, for example, the installation environment as the OS installer
  cannot instruct the firmware to boot only from the installed target, and not
  further USB sticks.

**It is extremely important that you use the most recent firmware with your
Morello board, to avoid these and other issues present in earlier firmware
releases.**
