# Introduction

[CheriBSD](https://www.cheribsd.org/) is a version of the open-source
[FreeBSD](https://www.freebsd.org/) operating system that has been extended to
use CHERI architectural protections.
CheriBSD is intended to:

- Provide a clean, in-depth illustration and template for how CHERI support
  can be integrated with a general-purpose, MMU-enabled operating system;
- Enable validation and evaluation of CHERI-extended processor architectures
  and microarchitectures;
- Act as an OS research platform for how operating systems can use CHERI;
- Support the development and evaluation of CHERI-enabled applications; and
- Provide a foundation for CHERI-enabled software demonstrations.

This guide will support you in getting up and running with CheriBSD on the
CHERI-RISC-V and Arm Morello platforms.
As platforms vary substantially, from instruction-set emulators and executable
formal models through to FPGA- and ASIC-based hardware implementations, there
are several paths that can be taken to reach a shell prompt and start
productive work.
You might find that you want to build CheriBSD from scratch, or simply use one
of our downloadable pre-compiled images.
You may want to run directly from a live image in an emulator, or
alternatively want to use an installer image to install onto an internal disk
in an Arm Morello-based server or workstation.
