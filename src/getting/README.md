# Getting CheriBSD

CheriBSD can be installed by [downloading](../downloading/) or
[building](../building/) one of two types of disk images:

- **Memstick images** that boot and automatically run `bsdinstall`, the FreeBSD
  installer, which can be used to prepare a filesystem on a disk, and then
  install CheriBSD onto it.
  These will typically be used on Arm Morello boards.

- **Live images** that boot CheriBSD to a login prompt for interactive use.
  These will typically be used on instruction-set emulators such as QEMU and
  the Arm Morello FVP, as well as on FPGA.
