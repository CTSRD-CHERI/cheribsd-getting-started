# Using PiKVM for managing and IPKVM of a Morello box

[PiKVM](https://pikvm.org/) is an open source project to turn a Raspberry Pi into a full-featured internet-connected keyboard-video-mouse (KVM) device for controlling computers.  It allows you to view the HDMI output of a computer in a web browser, allows remote keyboard and mouse through the browser, and also allows you to upload remote media such as USB stick and CD/DVD images.  This allows fully-remote 'lights out' management of a PC.  With a few configuration tweaks, it can also be used to manage a Morello board, including connecting to its various serial consoles.

## Required hardware

You can use one of the following hardware:  

* Any [official PiKVM hardware](https://pikvm.org/products/). _Recommended option_. This is supported by the PiKVM team and funds ongoing development. However it is relatively expensive and as a standalone box with separate PSU which doesn't integrate well in a server environment.

* A [PiKVM 'DIY v2'](https://docs.pikvm.org/v2/). A Raspberry Pi 4 or Zero2W with an HDMI to CSI adapter (examples include ['Geekworm C779'](https://wiki.geekworm.com/C779) and ['WaveShare'](https://www.waveshare.com/wiki/HDMI_to_CSI_Adapter)). You don't need the breadboard which generates ATX case control signals (power button and LEDs) as the Morello MCC handles this. Both need modifications to their USB wiring as described on the PiKVM v2 page.  The Zero2W is simpler to wire but only has wifi, no ethernet.

* Untested: Another Pi-based IPKVM from e.g. [SupTronics](https://suptronics.com/ite/ipkvm_pcie.html) or [BliKVM](https://www.blikvm.com/docs/development/modify-pikvm-image/) as found on AliExpress ([example X630-A8](https://www.aliexpress.com/item/1005007417520546.html)). These fit in a PCIe slot and some are powered via PoE. We haven't tested these and note that these clones don't fund PiKVM development and don't receive support from the PiKVM developers. 

* Any third party KVM board that does not use a Pi or Compute Module 4 *will not work*.

You will also need:

* USB-A to fullsize USB-B cable

* USB-A to your Pi's USB OTG port - can be either microUSB (Zero2W) or USB-C for most other machines

* 5 volt power supply and cable for your PiKVM (varies by model)

* HDMI cable (male to male)

* Micro SD card at least 32GB in size



## Connections

To get set up, you will need to connect as follows:

1. A cable from a USB-A port on the Pi to the USB-B port on the Morello 

2. A cable from a USB-A port on the Morello to the USB-C or MicroUSB data port on the PiKVM (check the documentation)

3. HDMI cable from the Morello to the PiKVM 

4. Do not connect any ATX headers for power/reset button control - they are not needed.

5. Power connection to the PiKVM

6. Power connection to the Morello box

## PiKVM setup

Follow [the PiKVM instructions about getting started with your PiKVM].  In brief:

1. Download the appropriate SD card image for your hardware [from the PiKVM website](https://docs.pikvm.org/flashing_os/) and write it to a micro SD card (use [Raspberry Pi Imager](https://www.raspberrypi.com/software/) but refuse customization options).

2. If using the DIY Zero2W you will likely need to [configure wifi](https://docs.pikvm.org/on_boot_config/) by putting the card into a computer and editing some settings files before first boot.  You can do this on other boards if you plan to use wifi instead of Ethernet.

3. Insert the card into the Pi (the slot is usually on the underside).

4. Power on the Pi and let it boot. 

5. Connect power to the Morello box and switch on the rear power switch but don't press the power button.

6. Visit the website hosted on the PiKVM in your web browser (e.g. http://pikvm.local/ or find its IP via your router/DHCP server)

Follow the [PiKVM cheat sheet](https://docs.pikvm.org/cheatsheet/) for how to interact with the PiKVM web interface and command line.  The PiKVM OS is based on ArchLinuxARM, a fork of Arch Linux.  At this point we have not booted the Morello so we'll have a black video window saying a problem with signal - this is expected.

Open the web terminal for the PiKVM and continue below.  You can alternatively access the terminal using SSH, as described in the cheatsheet.

### Filesystem modification

Note that the PiKVM filesystem is read only by default. To make config changes you will need to temporarily make it read-write: 

```
su # (enter PiKVM root password)  
rw 
# make changes here 
ro 
exit # back to unprivileged webterm user
```

### Serial port setup

First we'll do some serial port setup:

```
su
rw
pacman -S screen picocom # install Arch packages
usermod -aG uucp kvmd-webterm # allow web term user access to serial ports
ro 
reboot 
```

The PiKVM will reboot. Wait a few minutes then, in your browser, log back into the web terminal. 

### Screen terminal multiplexer

If you wish you can use GNU Screen to hold your serial sessions. Due to the read only filesystem it requires some tweaking each time you run screen:

```
mkdir -p /tmp/s && chmod 700 /tmp/s 
SCREENDIR=/tmp/s SHELL=bash screen 
```
 
When screen is running pressing Ctrl-A then the key C will create a new sub-session and Ctrl-A then 0 to Ctrl-A then 9 will flip between one of your running sessions. If you close your terminal window, adding `-r` to your screen command above will reconnect to the existing session, and if you didn't disconnect cleanly then it's `-r -d`

## Serial consoles

To connect to the Morello terminals, create two Screen sub-sessions (or two web browser tabs or SSH logins each showing the PiKVM console) and in the first run 

```
picocom -b 115200 -eb /dev/ttyUSB0 
```
 
which connects to the MCC console, and in the second:

```
picocom -b 115200 -eb /dev/ttyUSB2 
```
 
which connects to the Morello (and hence CheriBSD) console.  With the above commands, the keystrokes Ctrl-B followed by Ctrl-X can be used to quit picocom.

If you press Enter on the MCC console you should see a `Cmd>` prompt indicating the Morello MCC is communicating.  If not, check your wiring and that power is connected to the Morello.

## Upgrading the firmware via the PiKVM

You may need to upgrade the firmware on your Morello board.  This can be done from the PiKVM.  The process is covered in the [firmware upgrade section](../morello-firmware) which you should read thoroughly first. We only note here that the commands to mount the MCC's firmware USB mass storage FAT filesystem from the PiKVM web console and copy the firmware are:

```
su
rw
git clone https://git.morello-project.org/morello/board-firmware.git
mount /dev/sda1 /mnt
rm -rf /mnt/*
cp -R board-firmware/* /mnt
umount /mnt
sync
ro
exit
```

## Power control of the Morello

In the MCC console, `shutdown` will power down the Morello but keep the MCC console alive.  `reboot` will power up the Morello, or power cycle it if it is already on.

## Installing CheriBSD using the PiKVM remote media

We can use the PiKVM to present a virtual USB flash drive to the Morello to provide its install media.

First, download an image from the [CheriBSD](https://www.cheribsd.org/) website.  These are typically `xz` compressed - you will need to decompress them on your computer, to give a file with a `.img` extension, eg via:

```
unxz cheribsd-memstick-arm64-aarch64c-25.03.img.xz
```

Follow the [PiKVM instructions](https://docs.pikvm.org/msd/) about how to configure this as a 'flash drive', which is accessed from the Drive menu at the top of the PiKVM's 'KVM' web page.  In brief:

1. Open another browser tab on the PiKVM, and this time select 'KVM' instead of 'Terminal', and you should see 'Drive' at the top right. Click on it.

2. Click _Select an image to upload_, then _Browse_ and choose the `.img` file from your local storage.  Click _Upload_ and wait as the image uploads.

3. Then in the _Image_ dropdown, choose the name of your image.

4. Toggle _drive mode_ to _Flash_ and click _Connect drive to server_

5. The virtual USB flash drive has now been 'inserted' into the Morello.  You can _Disconnect_ it later when you are done installing.

## Installing CheriBSD

You can now proceed to install CheriBSD [according to the instructions](../morello-install).  In the Boot Manager menu you will need to select 'PiKVM Composite Device' to use the virtual flash drive.

When the installer has booted, you should see activity on both the Morello console and the PiKVM HDMI display.  You can install using either method.  In the HDMI display the keyboard should work as normal.

## PiKVM tweaks

When using the mouse, it is better to go to the PiKVM's System menu and select [_Mouse mode: relative_](https://docs.pikvm.org/mouse/).  This will cause your browser to capture mouse movement: you may need to press Escape or similar to exit the mouse capture.

If not talking to the PiKVM over a LAN you may find one of the [_Video mode_ options](https://docs.pikvm.org/video/) improves video performance.

### HDMI video modes

Different PiKVM hardware supports different video resolutions.  Pi4 and PiZero2W-based solutions with their single-lane CSI adapter only support up to 1920x1080 at 50Hz (aka '1080p') while CM4-based solutions support higher resolutions.  To avoid this, the default resolution of the HDMI capture is 1280x720.  You can change the HDMI resolution by [changing the EDID](https://docs.pikvm.org/edid/) of the 'monitor' the PiKVM pretends to be - this may need a reboot of the Morello to take effect.

