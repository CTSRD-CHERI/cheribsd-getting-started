# Userlevel process environments

The CheriBSD userspace likewise supports two different execution environments,
hybrid processes and CheriABI (pure-capability) processes:

- **Hybrid processes** provide strong binary compatibility with the non-CHERI
  version of the same architecture -- for example, aarch64 on Morello.
- **CheriABI processes** implement strong referential and spatial memory
  protection through the system-call interface, dynamic linker, language
  runtime including heap memory allocators, and compiler-generated code.
  This protects against memory memory-safety vulnerabilities in both system
  services and applications.
  CheriABI is described in an [ASPLOS 2019
  paper](https://www.cl.cam.ac.uk/research/security/ctsrd/pdfs/201904-asplos-cheriabi.pdf).

Both environments can be used over either of the hybrid or pure-capability
kernels.

Pre-compiled third-party software applications (packages) are provided for
both ABIs, although CheriABI packages are currently considered experimental.
This is discussed further in the [chapter on packages](../packages/).
