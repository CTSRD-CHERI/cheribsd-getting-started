# Third-party packages

CheriBSD on Morello ships with both hybrid and CheriABI packages
(compilations) of the FreeBSD ports collection, each targeting a different
form of code generation and Application Binary Interface (ABI).
They have different levels of completeness, maturity, security, and support.

**This document describes CheriBSD support for packages as of the 22.05
release, which differs substantially from prior releases and software
snapshots due to the introduction of a `pkg64c` package set.**

## Hybrid ABI packages

**Hybrid ABI packages** are compiled almost identically to packages in the
baseline non-CHERI architecture (e.g., Armv8-a for Morello, and 64-bit
RISC-V for RISC-V), and do not have improvements in memory protection or
software compartmentalization.
There are over 20,000 packages available.
These are considered *appropriate for day-to-day use*.

These packages are installed in the `/usr/local64` hierarchy. By default
`/usr/local64/bin` and `/usr/local64/sbin` should be in your path.

*Note:* Because these packages are installed in a non-standard location,
there may be bugs related to them looking in `/usr/local` instead of
`/usr/local64` for dependencies. Please report bugs of this sort in
the [CheriBSD ports issue tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).

## CheriABI packages

**CheriABI packages** are compiled using pure-capability CHERI C/C++, and
employ fine-grained C/C++ memory protection.
There are currently over 8,000 CheriABI packages available.
These packages are considered *experimental*.

These packages are installed in the standard `/usr/local` hierarchy.

*Note:* These packages compile, but many have CHERI-related warnings that
have not been audited and only a limited set have been tested.  Bugs
related to CHERI support can be reported in the
[CheriBSD ports issue tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).

## Managing packages in CheriBSD

*Note:* As of this writing we only provide packages for Morello systems.
We aim to add RISC-V package sets in the near future.

Hybrid and CheriABI packages can be installed via two independent instances of
the package system in CheriBSD, one for each ABI.
The intention is that, over time, the CheriABI packages will become more
mature, and hence the preferred collection for day-to-day use.

The default `pkg` command will print a note describing this arrangement, but
cannot be used for package management as described in FreeBSD documentation.
To manage and install hybrid packages, use the `pkg64` command.
To manage and install CheriABI packages, use the `pkg64c` command.
We expect that `pkg64c` will be renamed to `pkg` in a future CheriBSD release.

You can learn more about FreeBSD's package manager by reading the [pkg(8)
manual page](https://www.freebsd.org/cgi/man.cgi?pkg(7))
and the [FreeBSD Handbook chapter on packages and
ports](https://docs.freebsd.org/en/books/handbook/ports/).
