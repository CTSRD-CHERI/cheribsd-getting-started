# Installing CheriBSD

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

**TO WRITE**

## Using a live image

The specific command line to use for a live image will depend on the target
execution environment.
When using `cheribuild` to launch QEMU-CHERI or the Arm Morello FVP,
`cheribuild` will automatically use any necessary command-line arguments to
launch the most recently build live image in the emulator.
