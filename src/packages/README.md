# Third-party packages

CheriBSD on Morello ships with both hybrid and CheriABI packages
(compilations) of the FreeBSD ports collection, each targeting a different
form of code generation and Application Binary Interface (ABI).
They have different levels of completeness, maturity, security, and support.

- **Hybrid ABI packages** are compiled almost identically to packages in the
  baseline non-CHERI architecture (e.g., Armv8-a for Morello, and 64-bit
  RISC-V for RISC-V), and do not have improvements in memory protection or
  software compartmentalization.
  There are in the range of 20,000-30,000 hybrid ABI packages available.
  These are considered *appropriate for day-to-day use*.

- **CheriABI packages** are compiled using pure-capability CHERI C/C++, and
  employ fine-grained C/C++ memory protection.
  There are in the range of 1,000-3,000 CheriABI packages available.
  These are considered *experimental*.
  command.

Hybrid and CheriABI packages can be installed via two independent instances of
the package system in CheriBSD, one for each ABI.
The intention is that, over time, the CheriABI packages will become more
mature, and hence the preferred collection for day-to-day use.

The default `pkg` command will print a note describing this arrangement, but
cannot be used for package management as described in FreeBSD documentation.
To manage and install hybrid packages, use the `pkg64` command.
To manage and install CheriABI packages, use the `pkg64c` command.
We expect that `pkg64c` will be renamed to `pkg` in a future CheriBSD release.
