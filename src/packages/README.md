# 64-bit and CheriABI packages

CheriBSD on Morello ships with both hybrid and CheriABI packages
(compilations) of the FreeBSD ports collection.
The former is considered suitable for *day-to-day use* on Morello, providing
a large suite of open-source applications that make Morello boards usable
computers.
The latter is considered *experimental*, as, at this time, only a modest
portion of the ports collection builds for CheriABI, and they have seen little
validation.
Both hybrid and CheriABI packages can be installed via two independent
instances of the package system in CheriBSD, one for each ABI.
The intention is that, over time, the CheriABI packages will become more
mature, and hence the preferred collection for day-to-day use.

The default `pkg` command will print a note describing this arrangement, but
cannot be used for package management as described in FreeBSD documentation.
To manage and install hybrid packages, use the `pkg64` command.
To manage and install CheriABI packages, use the `pkg64c` command.
We expect that `pkg64c` will be renamed to `pkg` in a future CheriBSD release.
