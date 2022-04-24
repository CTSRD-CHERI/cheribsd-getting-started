# Downloading CheriBSD images

Two types of CheriBSD images may be download:

- *memstick images* that boot and automatically run `bsdinstall`, the FreeBSD
  installer, which can be used to prepare a filesystem on a disk, and then
  install CheriBSD onto it.
  These will typically be used on Arm Morello boards.

- *live images* that boot CheriBSD to a login prompt for interactive use.
  These will typically be used on instruction-set emulators such as QEMU and
  the Arm Morello FVP.

Both can be downloaded from the [CheriBSD website](https://www.cheribsd.org/).

## Writing a memstick image to a USB stick

*memstick* images are typically decompressed using the UNIX `unxz` command:

```
unxz cheribsd-memstick-dev-arm64-aarch64c-2022-04-05.img.xz
````

and then written to a USB stick using the UNIX `dd` command:

```
dd if=cheribsd-memstick-dev-arm64-aarch64c-2022-04-05.img of=/dev/DISK bs=1048576
```

Here, *DISK* should be replaced with the name of the appropriate device.

## Using a live image

Live images are typically decompressed using the UNIX `unxz` command:

```
unxz cheribsd-memstick-dev-arm64-aarch64c-2022-04-05.img.xz
````

The specific command line to use for a live image will depend on the target
execution environment.
