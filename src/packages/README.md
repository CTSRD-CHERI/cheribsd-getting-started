# Third-party packages

CheriBSD on Morello ships with CheriABI, hybrid ABI and benchmark ABI packages
(compilations) of the CheriBSD ports collection, each targeting a different
form of code generation and Application Binary Interface (ABI).
They have different levels of completeness, maturity, security, and support.

[CheriBSD ports](https://github.com/CTSRD-CHERI/cheribsd-ports)
extends FreeBSD ports, a collection of over 34,000 third-party
software adaptations to FreeBSD, with CHERI- and CheriBSD-specific patches.
The
[releng/24.05](https://github.com/CTSRD-CHERI/cheribsd-ports/tree/releng/23.11)
CheriBSD ports branch contains ports matching the packages built for the current
release.

## Overview

The following table presents an overview of available package managers in
CheriBSD that are described in more details in consecutive sections.
You can also browse package repositories at
[pkg.CheriBSD.org](https://pkg.CheriBSD.org/)
to check what packages are available for a specific ABI version.

| ABI           | #       | Manager   | Install path     | Suitable for           |
|---------------|---------|-----------|------------------|------------------------|
| CheriABI      | ~10,000 | `pkg64c`  | `/usr/local`     | Security evaluation    |
| Benchmark ABI | ~10,000 | `pkg64cb` | `/usr/local64cb` | Performance evaluation |
| Hybrid ABI    | ~25,000 | `pkg64`   | `/usr/local64`   | Day-to-day use         |

## Package managers in CheriBSD

*Note:* As of this writing we only provide packages for Morello systems.

CheriBSD includes three package managers:

* `pkg64c` for CheriABI packages;
* `pkg64cb` for benchmark ABI packages;
* `pkg64` for hybrid ABI packages.

The FreeBSD package manager `pkg` is not available on CheriBSD.
We expect that `pkg64c` will be renamed to `pkg` in a future CheriBSD release.
The intention is that, over time, the CheriABI packages will become more
mature, and hence the preferred collection for day-to-day use.

The syntax of the `pkg64*` commands match the syntax of the `pkg`
command from FreeBSD.
You can find information on package manager commands in FreeBSD's `pkg` manual
pages shipped with CheriBSD, e.g. for the commands `pkg64 rquery` and
`pkg64c rquery`, execute `man pkg-rquery`.

Additionally, you can learn more about FreeBSD's package manager by reading the
[pkg(8) manual page](https://www.freebsd.org/cgi/man.cgi?pkg(7)) online
and the [FreeBSD Handbook chapter on packages and
ports](https://docs.freebsd.org/en/books/handbook/ports/).

## CheriABI packages

**CheriABI packages** are compiled using pure-capability CHERI C/C++, and
employ fine-grained C/C++ memory protection.

These packages are considered appropriate for **experimental use** and
a **security evaluation** of CHERI-extended hardware-software stacks.
Their primary function is to provide necessary dependencies for efforts to port
software to CheriABI and to support CHERI demonstration and evaluation.
They are suitable for research and development of software that benefit from
spatial and temporal memory safety as well as software compartmentalisation.
They can also be used to investigate potential memory safety issues in
third-party software that are easier to detect and debug using a CHERI-enabled
hardware-software stack.

There are currently ~10,000 CheriABI packages available, including:

* Databases: `sqlite3`;

* Development utilities: `autoconf`, `chericat`, `cmake`, `git`, `gmake`;

* Editors: `nano`, `vim-tiny`;

* GUI environment: `cheri-desktop` (`seatd`, `sddm`, `wayland`,
  `plasma5-plasma-workspace`, `dolphin`, `kate`, `konsole`, `okular`);

* Hypervisor tools: `cheri-vm-support` (`u-boot-bhyve-arm64`);

* Language interpreters: `perl5`, `tcl86`;

* Libraries: `libnghttp2`, `libressl`, `libssh2`, `lzlib`;

* Networking tools: `rsync31`;

* Shells: `bash`, `dash`;

* System tools: `sudo`, `tmux`.

The packages are installed in the standard `/usr/local` hierarchy as they match
the base system ABI.

## Benchmark ABI packages

**Benchmark ABI packages** are compiled similarly to CheriABI packages, but
differ in generated code that takes into account current limitations of the
Morello architecture.
You should make sure to read the
[Benchmarking guidance](../benchmarking/)
section before using the benchmark ABI packages to understand the differences
between CheriABI and the benchmark ABI.

These packages are considered appropriate for a **performance evaluation** of
the Arm Morello architecture and third-party software executed on it.
They should not be used for a security evaluation, in which case you should use
CheriABI instead.

There are currently ~10,000 benchmark ABI packages available which should
include almost all packages that are available in the CheriABI package
repository.

The packages are installed in the `/usr/local64cb` hierarchy.
By default, `/usr/local64cb/bin` and `/usr/local64cb/sbin` are included in your
`PATH` environment variable of a shell shipped with CheriBSD.
If you are planning to use a custom shell, remember to add these paths to
`PATH`.

Different numbers of CheriABI and benchmark ABI packages is a result of using a
custom local base path (`/usr/local64cb`) and a custom `LD_LIBRARY_PATH`
variable name (`LD_64CB_LIBRARY_PATH`) for the benchmark ABI.
Multiple ports do not correctly handle these differences.

## Hybrid ABI packages

**Hybrid ABI packages** are compiled almost identically to packages in the
baseline non-CHERI architecture (e.g., Armv8-a for Morello, and 64-bit
RISC-V for RISC-V), and do not have improvements in memory protection or
software compartmentalization.

These packages are considered *appropriate for day-to-day use*.
They are intended to provide stable versions of tools necessary to develop
software and use your CHERI system while more software is ported to CheriABI.

There are currently ~25,000 hybrid ABI packages available, including:

* Browsers: `chromium`, `firefox`;

* Compilers:

  * `llvm-morello`;

    Includes Clang (a CHERI C/C++ compiler), LLD (a linker), and the LLVM
    infrastructure for the Arm Morello architecture.
    Binaries installed with this package have the suffix `-morello`.

  * `llvm`;

    Adds links in `/usr/local64/bin` to allow the `llvm-morello` package to be
    used as the default LLVM package, without the suffix `-morello`,
    e.g. `clang` instead of `clang-morello`.

  * `llvm-base`;

    Adds links in `/usr/bin` to allow the `llvm` package to be used in place of
    a base-system toolchain. The `cc` script installed with this package adds
    compiler flags required to natively compile code for a CHERI-enabled
    architecture.

  * `rust`.

* Debuggers: `gdb`;

* Development utilities: `meson`, `ninja`;

* Editors: `emacs`;

* Language interpreters: `lua54`, `python39`, `ruby`;

* Shells: `fish`, `zsh`.

The packages are installed in the `/usr/local64` hierarchy.
By default, `/usr/local64/bin` and `/usr/local64/sbin` are included in your
`PATH` environment variable of a shell shipped with CheriBSD.
If you are planning to use a custom shell, remember to add these paths to
`PATH`.
