# Installing on an Arm Morello Board

Once you have a disk image, you will either need to write it to a USB stick
to boot an Arm Morello system, or specify it as an argument to an emulator
such as QEMU-CHERI or the Arm Morello FVP.

## Writing an installer disk image to a USB stick

With appropriate substitutions of image filename and target device, the
following command would write the image to a USB stick for use with a Morello
board:

```
dd if=cheribsd-memstick-dev-arm64-aarch64c-2022-04-05.img of=/dev/DISK bs=1048576
```

It is also possible to write a live image to a USB stick, with appropriate
filename substitution.

## Installing an Arm Morello system

It is possible to use Arm Morello systems both by booting the live image on a
USB stick, and also by installing to the internal hard disk.
In this section, we describe the how to perform an installation using the
memstick image.

### Updating and configuring the Morello firmware

**It is important that the most recent Arm-provided firmware be installed on
your Morello board, as the board has most likely shipped with an older
firmware version.
If you are not running the latest version, you may experience hard-to-diagnose
behaviour or reliability problems.**

Newer versions contain important feature extensions (such as support for
network booting) and also bug fixes (such as persistent variable support for
UEFI, which is relied on for CheriBSD boot).
You should follow the directions provided in [Arm's Morello installation
guide](https://developer.arm.com/documentation/den0132/latest/), having
downloaded Arm's [prebuilt Morello firmware
binaries](https://git.morello-project.org/morello/board-firmware).
You can find detailed instructions for updating the firmware on the built-in
firmware SD card
[here](https://developer.arm.com/documentation/den0132/0100/Flash-the-onboard-SD-card).

### Connecting to the serial consoles

Multiple consolers are available via the USB debug port on an Arm Morello
board, including to the management microcontrollers, and also the console UART
for the application processor.
These typically attach as USB serial ports 0 (management console) and 2
(Morello console), and a serial connection program such as `cu` can be used to
connect to them.
For example, on a FreeBSD system, the following commands can be used to
connect:

- Management console: `cu -l /dev/cuaU0 -s 115200`
- Morello console: `cu -l /dev/cuaU2 -s 115200`

However, commands and device names will vary by operating system.

### Starting the boot

Before powering on your Morello board, ensure that the installer USB stick is
inserted into one of the USB ports on the board.
Use the power switch to power the board on, which should lead to firmware
output on the management console during a multi-minute initialization and
boot cycle.
Once that has completed, you will see Morello firmware output on the Morello
console, followed by CheriBSD loader and kernel boot output.
You will eventually be prompted to select a terminal emulation type:

```
Welcome to FreeBSD!

Please choose the appropriate terminal type for your system.
Common console types are:
   ansi     Standard ANSI terminal
   vt100    VT100 or compatible terminal
   xterm    xterm terminal emulator (or compatible)
   cons25w  cons25w terminal

Console type [xterm]:
```

In most cases, simply hitting Enter will be the right thing to do.
This will enter `bsdinstall`, the interactive FreeBSD installer.

### bsdinstall

`bsdinstall` uses the `libdialog` text interface library.
**Be aware that you will sometimes need to use the space bar, and not enter
key, to select menu options, which many users find confusing.**

Select **Install** at the first menu by hitting Enter:
```
                     ┌────────────Welcome──────────────┐
                     │ Welcome to FreeBSD! Would you   │
                     │ like to begin an installation   │
                     │ or use the live CD?             │
                     ├─────────────────────────────────┤
                     │ <Install> < Shell > <Live CD>   │
                     └─────────────────────────────────┘
```

#### Keymap selection

If you intend only to use the serial console, and not video console, select
the default keymap by hitting Enter at the next menu; otherwise, use the
cursor keys and space bar to select a country-specific keyboard layout:
```
     ┌───────────────────────Keymap Selection──────────────────────────┐
     │ The system console driver for FreeBSD defaults to standard "US" │
     │ keyboard map. Other keymaps can be chosen below.                │
     │ ┌─────────────────────────────────────────────────────────────┐ │
     │ │>>> Continue with default keymap                             │ │
     │ │->- Test default keymap                                      │ │
     │ │( ) Armenian phonetic layout                                 │ │
     │ │( ) Belarusian                                               │ │
     │ │( ) Belgian                                                  │ │
     │ │( ) Belgian (accent keys)                                    │ │
     │ │( ) Brazilian (accent keys)                                  │ │
     │ │( ) Brazilian (without accent keys)                          │ │
     │ │( ) Bulgarian (BDS)                                          │ │
     │ │( ) Bulgarian (Phonetic)                                     │ │
     │ │( ) Canadian Bilingual                                       │ │
     │ └────↓(+)─────────────────────────────────────────────12%─────┘ │
     ├─────────────────────────────────────────────────────────────────┤
     │                   <Select>          <Cancel>                    │
     └──────────────────[Press arrows, TAB or ENTER]───────────────────┘
```

#### Hostname

Press Enter to accept the default hostname, or replace it:
```
           ┌───────────────────Set Hostname──────────────────────┐
           │ Please choose a hostname for this machine.          │
           │                                                     │
           │ If you are running on a managed network, please ask │
           │ your network administrator for an appropriate name. │
           │ ┌─────────────────────────────────────────────────┐ │
           │ │cheribsd                                         │ │
           │ └─────────────────────────────────────────────────┘ │
           ├─────────────────────────────────────────────────────┤
           │                     <  OK  >                        │
           └─────────────────────────────────────────────────────┘
```

#### Partitioning (automatic or manual)

Press Enter to select an automated UFS install:
```
          ┌─────────────────────Partitioning───────────────────────┐
          │ How would you like to partition your disk?             │
          │ ┌────────────────────────────────────────────────────┐ │
          │ │   Auto (UFS)  Guided UFS Disk Setup                │ │
          │ │   Manual      Manual Disk Setup (experts)          │ │
          │ │   Shell       Open a shell and partition by hand   │ │
          │ │                                                    │ │
          │ │                                                    │ │
          │ └────────────────────────────────────────────────────┘ │
          ├────────────────────────────────────────────────────────┤
          │               <  OK  >        <Cancel>                 │
          └────────────────────────────────────────────────────────┘
```

#### Partitioning (disk selection)

Select the internal SATA hard disk, which will typically be ada0 on Morello
boards:
```
       ┌───────────────────────Partitioning──────────────────────────┐
       │ Select the disk on which to install FreeBSD.                │
       │ ┌─────────────────────────────────────────────────────────┐ │
       │ │   ada0  224 GB ATA Hard Disk <WDC WDS240G2G0A-00JH30>   │ │
       │ │   da0   57 GB Disk <SanDisk Ultra USB 3.0>              │ │
       │ │                                                         │ │
       │ │                                                         │ │
       │ └─────────────────────────────────────────────────────────┘ │
       ├─────────────────────────────────────────────────────────────┤
       │                 <  OK  >         <Cancel>                   │
       └─────────────────────────────────────────────────────────────┘
```

#### Partition (entire disk)

Select Entire Disk and press Enter:

```
                 ┌───────────────Partition──────────────────┐
                 │ Would you like to use this entire disk   │
                 │ (ada0) for FreeBSD or partition it to    │
                 │ share it with other operating systems?   │
                 │ Using the entire disk will erase any     │
                 │ data currently stored there.             │
                 ├──────────────────────────────────────────┤
                 │     <Entire Disk>   < Partition >        │
                 └──────────────────────────────────────────┘
```

#### Partition (entire disk - confirmation)

Press Enter to confirm:
```
                 ┌───────────────Partition──────────────────┐
                 │ Would┌──────Confirmation─────────┐disk   │
                 │ (ada0│ This will erase the disk. │  o    │
                 │ share│ Are you sure you want to  │  s?   │
                 │ Using│ proceed?                  │       │
                 │ data ├───────────────────────────┤       │
                 ├──────│     < Yes >   < No  >     │  ─────┤
                 │     <└───────────────────────────┘       │
                 └────────                             ─────┘
```

#### Partition Editor

Review the proposed partition scheme, select Finish, and then press Enter to
proceed:
```
           ┌──────────────────Partition Editor────────────────────┐
           │ Please review the disk setup. When complete, press   │
           │ the Finish button.                                   │
           │┌────────────────────────────────────────────────────┐│
           ││ada0            224 GB  GPT                         ││
           ││  ada0p1        260 MB  efi            /boot/ef     ││
           ││  ada0p2        220 GB  freebsd-ufs    /            ││
           ││  ada0p3        3.6 GB  freebsd-swap   none         ││
           ││da0             57 GB   GPT                         ││
           ││  da0p1         33 MB   efi                         ││
           ││  da0p2         814 MB  freebsd-ufs                 ││
           ││                                                    ││
           │└────────────────────────────────────────────────────┘│
           ├──────────────────────────────────────────────────────┤
           │<Create> <Delete> <Modify> <Revert> < Auto > <Finish> │
           └──────────────────────────────────────────────────────┘
```

#### Partition editor - confirmation

Select Commit and press Enter to continue with writing out a new partition
table:
```
           ┌──────────────────Partition Editor────────────────────┐
           │ Please review the disk setup. When complete, press   │
           │ the Finish button.                                   │
           │┌────────────────────────────────────────────────────┐│
           │┌──────────────────Confirmation─────────────────────┐││
           ││ Your changes will now be written to disk. If you  │
           ││ have chosen to overwrite existing data, it will   │
           ││ be PERMANENTLY ERASED. Are you sure you want to   │
           ││ commit your changes?                              │
           │├───────────────────────────────────────────────────┤
           ││ <   Commit    > <Revert & Exit> <    Back     >   │
           │└───────────────────────────────────────────────────┘
           │└─
           ├──────────────────────────────────────────────────────┤
           │<Create> <Delete> <Modify> <Revert> < Auto > <Finish> │
           └──────────────────────────────────────────────────────┘
```

#### Setting a root password

Enter a new root password and press Enter.
Then confirm it by typing the same password again and pressing Enter.

```
Please select a password for the system management account (root):
Typed characters will not be visible.
Changing local password for root
New Password:
```

#### Network configuration

If desired, configure Ethernet networking by pressing Enter.

```
  ┌────────────────────────Network Configuration───────────────────────────┐
  │ Please select a network interface to configure:                        │
  │ ┌────────────────────────────────────────────────────────────────────┐ │
  │ │   re0  RealTek 8168/8111 B/C/CP/D/DP/E/F/G PCIe Gigabit Ethernet   │ │
  │ │                                                                    │ │
  │ │                                                                    │ │
  │ └────────────────────────────────────────────────────────────────────┘ │
  ├────────────────────────────────────────────────────────────────────────┤
  │                     <  OK  >           <Cancel>                        │
  └────────────────────────────────────────────────────────────────────────┘
```

#### Network configuration - enabling IPv4

If desired, enable IPv4 on the Ethernet interface by selecting Yes and
pressing Enter:
```
                        ┌──Network Configuration────┐
                        │ Would you like to         │
                        │ configure IPv4 for this   │
                        │ interface?                │
                        ├───────────────────────────┤
                        │     < Yes >   < No  >     │
                        └───────────────────────────┘
```

#### Network configuration - DHCP for IPv4

If you will be using DHCP, select Yes and press Enter.
Otherwise select No and press Enter to perform a manual IPv4 configuration.

```
                        ┌──Network Configuration────┐
                        │ Would you like to use     │
                        │ DHCP to configure this    │
                        │ interface?                │
                        ├───────────────────────────┤
                        │     < Yes >   < No  >     │
                        └───────────────────────────┘
```

#### Network configuration - enabling IPv6

If desired, enable IPv6 on the Ethernet interface by selecting Yes and
pressing Enter:
```
                        ┌──Network Configuration────┐
                        │ Would you like to         │
                        │ configure IPv6 for this   │
                        │ interface?                │
                        ├───────────────────────────┤
                        │     < Yes >   < No  >     │
                        └───────────────────────────┘
```

#### Network configuration - SLAAC for IPv6

Press Enter to use stateless address autoconfiguration for IPv6:
```
                        ┌──Network Configuration────┐
                        │ Would you like to try     │
                        │ stateless address         │
                        │ autoconfiguration         │
                        │ (SLAAC)?                  │
                        ├───────────────────────────┤
                        │     < Yes >   < No  >     │
                        └───────────────────────────┘
```

#### Network configuration - resolver configuration

Enter DNS resolution configuration information, or press Enter to confirm
autoconfigured DNS configuration:
```
   ┌───────────────────────Network Configuration──────────────────────────┐
   │ Resolver Configuration                                               │
   │ ┌──────────────────────────────────────────────────────────────────┐ │
   │ │Search                                                            │ │
   │ │IPv6 DNS #1                                                       │ │
   │ │IPv6 DNS #2                                                       │ │
   │ │IPv4 DNS #1    192.168.4.1                                        │ │
   │ │IPv4 DNS #2                                                       │ │
   │ └──────────────────────────────────────────────────────────────────┘ │
   ├──────────────────────────────────────────────────────────────────────┤
   │                     <  OK  >           <Cancel>                      │
   └──────────────────────────────────────────────────────────────────────┘
```

#### Local or UTC clock

Press Enter to select a system clock on UTC
```
   ┌───────────┤Select local or UTC (Greenwich Mean Time) clock├───────────┐
   │ Is this machine's CMOS clock set to UTC?  If it is set to local time, │
   │ or you don't know, please choose NO here!                             │
   │                                                                       │
   ├───────────────────────────────────────────────────────────────────────┤
   │                          [ Yes  ]   [  No  ]                          │
   └───────────────────────────────────────────────────────────────────────┘
```

#### Timezone selection

Select your continent and press Enter:
```
                     ┌───────┤Time Zone Selector├───────┐
                     │ Select a region                  │
                     │ ┌──────────────────────────────┐ │
                     │ │ 1 Africa                     │ │
                     │ │ 2 America -- North and South │ │
                     │ │ 3 Antarctica                 │ │
                     │ │ 4 Asia                       │ │
                     │ │ 5 Atlantic Ocean             │ │
                     │ │ 6 Australia                  │ │
                     │ │ 7 Europe                     │ │
                     │ │ 8 Indian Ocean               │ │
                     │ │ 9 Pacific Ocean              │ │
                     │ │ 0 UTC                        │ │
                     │ └──────────────────────────────┘ │
                     ├──────────────────────────────────┤
                     │        [  OK  ]   [Cancel]       │
                     └──────────────────────────────────┘
```

Then select your country and press Enter:
```
        ┌────────────────────┤Countries in Europe├────────────────────┐
        │ Select a country or region                                  │
        │ ┌─^^^─────────────────────────────────────────────────────┐ │
        │ │ 35 Poland                                               │ │
        │ │ 36 Portugal                                             │ │
        │ │ 37 Romania                                              │ │
        │ │ 38 Russian Federation                                   │ │
        │ │ 39 San Marino                                           │ │
        │ │ 40 Serbia                                               │ │
        │ │ 41 Slovakia                                             │ │
        │ │ 42 Slovenia                                             │ │
        │ │ 43 Spain                                                │ │
        │ │ 44 Svalbard and Jan Mayen                               │ │
        │ │ 45 Sweden                                               │ │
        │ │ 46 Switzerland                                          │ │
        │ │ 47 Turkey                                               │ │
        │ │ 48 Ukraine                                              │ │
        │ │ 49 United Kingdom of Great Britain and Northern Ireland │ │
        │ │ 50 Åland Islands                                        │ │
        │ └────────────────────────────────────────────────100%─────┘ │
        ├─────────────────────────────────────────────────────────────┤
        │                     [  OK  ]   [Cancel]                     │
        └─────────────────────────────────────────────────────────────┘
```

Confirm your choice by selecting Yes and pressing Enter:
```
   ┌────────────────────────────┤Confirmation├────────────────────────────┐
   │ Does the abbreviation `BST' look reasonable?                         │
   ├──────────────────────────────────────────────────────────────────────┤
   │                          [ Yes  ]   [  No  ]                         │
   └──────────────────────────────────────────────────────────────────────┘
```

#### Setting the date and time

As desired, set the date, or select Skip and press Enter if you plan
to use network time synchronization:
```
                   ┌────────────Time & Date───────────────┐
                   │                                      │
                   │  Month            Year               │
                   │  ┌───────────────┐┌───────────────┐  │
                   │  │May            ││2022           │  │
                   │  └───────────────┘└───────────────┘  │
                   │  ┌─────↑(-)───────────────────────┐  │
                   │  │    Sun Mon Tue Wed Thu Fri Sat │  │
                   │  │ 18   1   2   3   4   5   6   7 │  │
                   │  │ 19   8   9  10  11  12  13  14 │  │
                   │  │ 20  15  16  17  18  19  20  21 │  │
                   │  │ 21  22  23  24  25  26  27  28 │  │
                   │  │ 22  29  30  31                 │  │
                   │  │                                │  │
                   │  └─────↓(+)───────────────────────┘  │
                   ├──────────────────────────────────────┤
                   │       <Set Date>    <  Skip  >       │
                   └──────────────────────────────────────┘
```

And, likewise, the time:
```
                   ┌────────────Time & Date───────────────┐
                   │                                      │
                   │           ┌──┐ ┌──┐ ┌──┐             │
                   │           │00│:│34│:│36│             │
                   │           └──┘ └──┘ └──┘             │
                   │                                      │
                   ├──────────────────────────────────────┤
                   │       <Set Time>    <  Skip  >       │
                   └──────────────────────────────────────┘
```

#### System configuration

As desired, enable any further services (e.g., `ntpd` and `ntpdate`) by
selecting them and hitting Space:
```
  ┌────────────────────────System Configuration───────────────────────────┐
  │ Choose the services you would like to be started at boot:             │
  │ ┌───────────────────────────────────────────────────────────────────┐ │
  │ │ [ ] local_unbound  Local caching validating resolver              │ │
  │ │ [*] sshd           Secure shell daemon                            │ │
  │ │ [ ] moused         PS/2 mouse pointer on console                  │ │
  │ │ [ ] ntpdate        Synchronize system and network time at bootime │ │
  │ │ [ ] ntpd           Synchronize system and network time            │ │
  │ │ [ ] powerd         Adjust CPU frequency dynamically if supported  │ │
  │ │ [*] dumpdev        Enable kernel crash dumps to /var/crash        │ │
  │ │                                                                   │ │
  │ │                                                                   │ │
  │ └───────────────────────────────────────────────────────────────────┘ │
  ├───────────────────────────────────────────────────────────────────────┤
  │                               <  OK  >                                │
  └───────────────────────────────────────────────────────────────────────┘
```

Then press Enter to continue.

#### Add user accounts

If desired, select Yes and press Enter to add non-root accounts.
Otherwise, select No and press Enter

#### Final configuration

If there are further configuration settings to change, select an appropriate
option from the menu and press Space.
Otherwise, press Enter to complete the installation:
```
 ─┌─────────────────────────Final Configuration───────────────────────────┐────
  │ Setup of your FreeBSD system is nearly complete. You can now modify   │
  │ your configuration choices. After this screen, you will have an       │
  │ opportunity to make more complex changes using a shell.               │
  │ ┌───────────────────────────────────────────────────────────────────┐ │
  │ │   Exit              Apply configuration and exit installer        │ │
  │ │   Add User          Add a user to the system                      │ │
  │ │   Root Password     Change root password                          │ │
  │ │   Hostname          Set system hostname                           │ │
  │ │   Network           Networking configuration                      │ │
  │ │   Services          Set daemons to run on startup                 │ │
  │ │   System Hardening  Set security options                          │ │
  │ │   Time Zone         Set system timezone                           │ │
  │ │   Handbook          Install FreeBSD Handbook (requires network)   │ │
  │ │                                                                   │ │
  │ │                                                                   │ │
  │ └───────────────────────────────────────────────────────────────────┘ │
  ├───────────────────────────────────────────────────────────────────────┤
  │                               <  OK  >                                │
  └───────────────────────────────────────────────────────────────────────┘
```

#### Manual configuration

If desired, select Yes and press Enter to drop to a shell before reboot.
Otherwise, select No and press Enter.
```
                   ┌───────Manual Configuration──────────┐
                   │ The installation is now finished.   │
                   │ Before exiting the installer, would │
                   │ you like to open a shell in the new │
                   │ system to make any final manual     │
                   │ modifications?                      │
                   ├─────────────────────────────────────┤
                   │         < Yes >     < No  >         │
                   └─────────────────────────────────────┘
```

### Complete

Select Reboot and press Enter:
```
                        ┌─────────Complete───────────┐
                        │ Installation of FreeBSD    │
                        │ complete! Would you like   │
                        │ to reboot into the         │
                        │ installed system now?      │
                        ├────────────────────────────┤
                        │   <Reboot >  <Live CD>     │
                        └────────────────────────────┘
```

### Completing the installation

Remove the USB stick to prevent the installer from running after a system
reboot.
This is due to a [Morello firmware bug](../issues/) tht prevents proper
restoring system boot-disk state after a reboot.

Due to a [Morello firmware bug](../issues/), OS-triggered reboot is not
reliable.
Until a firmware revision correcting this bug is released, you will need to
type `reboot` on the management console to reboot after installation.

### Logging in

Log in using the username `root` and the password you entered during the
installation process:

```
...
Mounting late filesystems:.
Security policy loaded: MAC/ntpd (mac_ntpd)
Starting ntpd.
Starting cron.
Starting sendmail_submit.
Starting sendmail_msp_queue.
Performing sanity check on sshd configuration.
Starting sshd.
Starting background file system checks in 60 seconds.

Sun May  8 00:13:04 BST 
CheriBSD/arm64 (cheribsd) (ttyu0)

login: root
Password:
May  8 00:13:25 cheribsd login[772]: ROOT LOGIN (root) ON ttyu0
Last login: Sat May  7 23:41:04 on ttyu0
FreeBSD 14.0-CURRENT #0 dev-d4897febcde: Tue May  3 13:34:49 BST 2022     rnw24@zeno.sec.cl.cam.ac.uk:/home/rnw24/cheri/build/cheribsd-morello-purecap-build/home/rnw24/cheri/cheribsd/arm64.aarch64c/sys/GENERIC-MORELLO

Welcome to CheriBSD!

CheriBSD extends FreeBSD to implement memory protection and software
compartmentalization features available in CHERI-extended CPUs.

We provide support via a mailing list:
  https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/cheri-lists.html

And also via the CHERI-CPU Slack:
  https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/cheri-slack.html

CheriBSD source may be found at:
  https://github.com/CTSRD-CHERI/cheribsd/

Find out more about about CHERI at https://cheri-cpu.org/
You have new mail.
root@cheribsd:~ # 
```

You can now add further users, install SSH keys for remote access, install
packages, and so on.
