# CheriBSD features

CheriBSD extends FreeBSD with added CHERI-enabled features, such as kernel and
userspace memory safety.
Some of these features are in the main CheriBSD branch; other experimental
features  remain on development branches.

## Kernel compilation modes

The CheriBSD kernel can be compiled either as hybrid or pure-capability code:

- The **hybrid kernel** enables capaility use in userspace while making
  relatively little use of capabilities in the kernel's own implementation.
- The **pure-capability kernel** implements strong referential and spatial
  memory protection internally in the kernel, protecting against memory-safety
  vulnerabilities in components such as the network stack and system-call
  layer.

## Userspace process environments

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

## CheriABI desktop environment (experimental)

As of the 22.12 release, the installer has gained the option to install a
desktop environment using the Mali Bifrost GPU on the Morello System-on-Chip.
The option installs a basic desktop environment using KDE and Wayland
compiled for CheriABI with the `cheri-desktop` package.  It also
installs a hybrid ABI Firefox web browser via the `cheri-desktop-hybrid-extras`
package.

## Userspace heap temporal memory safety (experimental)

CheriBSD implements, on an experimental development branch, support for the
Cornucopia heap temporal safety algorithm, as well as successor algorithms
based on load-side-barrier features present in the Morello prototype
architecture and processor design.
Cornucopia is described in an [IEEE SSP 2020
paper](https://www.cl.cam.ac.uk/research/security/ctsrd/pdfs/2020oakland-cornucopia.pdf).

## Software compartmentalization (experimental)

CheriBSD implements two different
CHERI-enabled software compartmentalization models:

- **Colocated processes (co-processes) compartmentalization** accelerates
  UNIX Inter-Process Communication (IPC) and context switching by colocating
  multiple processes in the same address space, separating them using CHERI
  capabilities.  Support for co-processes is maintained in an experimental
  development branch and is not included in current software releases.
  For more information see the [Colocation Tutorial](https://github.com/CTSRD-CHERI/cheripedia/wiki/Colocation-Tutorial)
  wiki page.
- **Dynamic-linker-based compartmentalization** isolates shared libraries
  within a process using CHERI capabilities limiting the access of attackers
  who have achieved arbitrary code execution within a library.
  Initial support for linker-based library compartmentalization is included
  in the 22.12 release of CheriBSD.  See the compartmentalization(7) manual
  page on an installed system for more information.

## Virtualization (experimental)

CheriBSD implements, on an experimental development branch, CHERI extensions
to FreeBSD's bhyve Type 2 hypervisor on the Morello architecture.
This allows bhyve to host CHERI-enabled virtual machines, including those
running CheriBSD.
