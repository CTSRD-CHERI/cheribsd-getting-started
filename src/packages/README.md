# Third-party packages

CheriBSD on Morello ships with both hybrid ABI and CheriABI packages
(compilations) of the FreeBSD ports collection, each targeting a different
form of code generation and Application Binary Interface (ABI).
They have different levels of completeness, maturity, security, and support.

## Overview

The following table presents an overview of available package managers in
CheriBSD that are described in more details in consecutive sections.

| ABI        | #       | Manager | Install path   | Suitable for     | Examples |
|------------|---------|---------|----------------|------------------|----------|
| Hybrid ABI | ~20,000 | `pkg64` | `/usr/local64` | Day-to-day use   | bash<br/>gdb-cheri<br/>llvm-base<br/>vim |
| CheriABI   | ~8,000  | `pkg64c`| `/usr/local`   | Experimental use | git<br/>nano<br/>rsync<br/>sudo |

## Package managers in CheriBSD

*Note:* As of this writing we only provide packages for Morello systems.
We aim to add CHERI-RISC-V package sets in the near future.

CheriBSD includes two package managers:
* `pkg64` for hybrid ABI packages;
* `pkg64c` for CheriABI packages.

The FreeBSD package manager `pkg` is not available on CheriBSD.
We expect that `pkg64c` will be renamed to `pkg` in a future CheriBSD release.
The intention is that, over time, the CheriABI packages will become more
mature, and hence the preferred collection for day-to-day use.

The syntax of the `pkg64` and `pkg64c` commands match the syntax of the `pkg`
command from FreeBSD.
You can find information on package manager commands in FreeBSD's `pkg` manual
pages shipped with CheriBSD, e.g. for the commands `pkg64 rquery` and
`pkg64c rquery`, execute `man pkg-rquery`.

Additionally, you can learn more about FreeBSD's package manager by reading the
[pkg(8) manual page](https://www.freebsd.org/cgi/man.cgi?pkg(7)) online
and the [FreeBSD Handbook chapter on packages and
ports](https://docs.freebsd.org/en/books/handbook/ports/).

## Hybrid ABI packages

**Hybrid ABI packages** are compiled almost identically to packages in the
baseline non-CHERI architecture (e.g., Armv8-a for Morello, and 64-bit
RISC-V for RISC-V), and do not have improvements in memory protection or
software compartmentalization.

These packages are considered *appropriate for day-to-day use*.
They are intended to provide stable versions of tools necessary to develop
software and use your CHERI system while more software is ported to CheriABI.

There are currently over 20,000 hybrid ABI packages available, including:
* Compilers:
  * `llvm-morello`;

    Includes Clang (a CHERI C/C++ compiler), LLD (a linker), and the LLVM
    infrastructure for the Arm Morello architecture.
    Binaries installed with this package have the suffix `-morello`.

  * `llvm`;

    Adds links in `/usr/local64/bin` to allow the `llvm-morello` package to be
    used as the default LLVM package, without the suffix `-morello`,
    e.g. `clang` instead of `clang-morello`.

  * `llvm-base`.

    Adds links in `/usr/bin` to allow the `llvm` package to be used in place of
    a base-system toolchain. The `cc` script installed with this package adds
    compiler flags required to natively compile code for a CHERI-enabled
    architecture.
* Debuggers: `gdb-cheri`;
* Editors: `nano`, `vim`;
* Shells: `bash`, `dash`, `fish`.

The packages are installed in the `/usr/local64` hierarchy.
By default `/usr/local64/bin` and `/usr/local64/sbin` should be included in your
`PATH` environment variable of a default shell shipped with CheriBSD.
If you are planning to use a custom shell, remember to add these paths to
`PATH`.

*Note:* Because these packages are installed in a non-standard location,
there may be bugs related to them looking in `/usr/local` instead of
`/usr/local64` for dependencies. Please report bugs of this sort in
the [CheriBSD ports issue tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).

## CheriABI packages

**CheriABI packages** are compiled using pure-capability CHERI C/C++, and
employ fine-grained C/C++ memory protection.

These packages are considered *appropriate for experimental use*.
Their primary function is to provide necessary dependencies for efforts to port
software to CheriABI and to support CHERI demonstration and evaluation.
They are suitable for research and development of software that benefit from
spatial and temporal memory safety as well as software compartmentalisation.
They can also be used to investigate potential memory safety issues in
third-party software that are easier to detect and debug using a CHERI-enabled
hardware-software stack.

There are currently over 8,000 CheriABI packages available, including:
* Development utilities: `git`;
* Editors: `nano`;
* Libraries: `libnghttp2`, `libressl`, `libssh2`, `lzlib`, `wolfssl`;
* Networking tools: `rsync`;
* Security tools: `sudo`;
* Shells: `dash`.

The packages are installed in the standard `/usr/local` hierarchy as they match
the base system ABI.

*Note:* These packages compile, but many have CHERI-related warnings that
have not been audited and only a limited set have been tested. Bugs related to
CHERI support can be reported in the [CheriBSD ports issue
tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).
While the CheriABI packages are more interesting as they use CHERI memory-safety
features, you must remember that they might break in run-time and hence might
not be suitable for critical operations.
In such case, a corresponding hybrid ABI package might be considered instead of
a CheriABI package.
