# Background

## CHERI

**CHERI** is an extension to processor Instruction-Set Architectures (ISAs) to
introduce support for fine-grained memory protection and software
compartmentalization.
This is done by introducing a new architectural data type, the **CHERI
capability**, which can be used to implement pointers with strong integrity,
provenance validity, spatial safety, and monotonity.
CHERI requires new microarchitectural support, including support for memory
tags that triack the provenance validity of capabilities in registers and
memory.
CHERI extensions have been specified for:

- Arm's 64-bit Armv8-a ISA, with the prototype architecture known as
  **[Morello](https://www.arm.com/architecture/cpu/morelloMorello)**.
  The multi-GHz, multi-core, superscalar Morello design is a CHERI-extended
  version of the Arm Neoverse N1, available on an evaluation board from Arm.
- The open-source 32-bit and 64-bit RISC-V ISA, with the extended ISA known as
  **[CHERI-RISC-V](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-951.pdf)**.
  CHERI-RISC-V is available via multiple open-source soft processor cores for
  use on FPGA.

ISA-level emulation platforms exist for both architectures (QEMU for
CHERI-RISC-V and Morello, and Arm's Morello FVP for Morello).
Unless you have access to a Morello board or a high-end (and supported) FPGA
development platform, using
[QEMU-CHERI](https://www.cl.cam.ac.uk/research/security/ctsrd/cheri/cheri-qemu.html)
will be the easiest way to get started using CHERI.
This guide covers all of these use cases.

You can learn more about CHERI by reading the technical report, [An
Introduction to
CHERI](https://www.cl.cam.ac.uk/techreports/UCAM-CL-TR-941.pdf).

## CHERI C/C++

**CHERI C** and **CHERI C++** are programming-language dialects closely tied
to CHERI-based code generation and Application Binary Interfaces (ABIs) that
directly support referential and spatial memory safety.
These language variants compile to *pure-capability code*, which implements
all language- and sub-language pointers using CHERI capabilities rather than
architectural integers.
With suitable OS support, they can also support temporal memory safety.

## CheriBSD

**CheriBSD** is an extended version of the open-source
*[FreeBSD](https://www.freebsd.org/)* operating system.
FreeBSD is a mature UNIX-based operating system able to run on multiple
hardware architectures, and widely used in industry in service, appliance, and
embedded environments.
CheriBSD contains various CHERI-based extensions including support for kernel
and userspace memory safety, and supports multiple software
compartmentalization models.
CheriBSD runs on both CHERI-RISC-V and Morello with nearly identical feature
sets (Morello includes hardware virtualisation extensions not present in the
CHERI-RISC-V cores, and so CHERI-extended bhyve hypervisor support is not
available for CHERI-RISC-V).
