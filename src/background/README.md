# Background: CHERI and CheriBSD

## CHERI

*CHERI* is an extension to processor Instruction-Set Architectures (ISAs) to
introduce support for fine-grained memory protection and software
compartmentalization.
CHERI extensions have been specified for:

- Arm's 64-bit Armv8-a ISA, with the prototype architecture known as
  *[Morello](https://www.arm.com/architecture/cpu/morelloMorello)*.
  The multi-GHz, multi-core, superscalar Morello design is a CHERI-extended
  version of the Arm Neoverse N1, available on an evaluation board from Arm.
- The open-source 32-bit and 64-bit RISC-V ISA, with the extended ISA known as
  CHERI-RISC-V.
  CHERI-RISC-V is available via multiple open-source soft processor cores for
  use on FPGA.

ISA-level emulation platforms exist for both architectures (QEMU for
CHERI-RISC-V and Morello, and Arm's Morello FVP for Morello).

## CheriBSD

*CheriBSD* is an extended version of the open-source
*[FreeBSD](https://www.freebsd.org/)* operating system.
FreeBSD is a mature UNIX-based operating system able to run on multiple
hardware architectures, and widely used in industry in service, appliance, and
embedded environments.
CheriBSD contains various CHERI-based extensions including support for kernel
and userspace memory safety, and for multiple software compartmentalization
models.
CheriBSD runs on both CHERI-RISC-V and Morello with nearly identical feature
sets (Morello includes hardware virtualisation extensions not present in the
CHERI-RISC-V cores, and so CHERI-extended bhyve hypervisor support is not
available for CHERI-RISC-V).
