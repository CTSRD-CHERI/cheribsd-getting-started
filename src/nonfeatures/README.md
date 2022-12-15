# Unsupported FreeBSD features

Certain FreeBSD features are currently unavailable in CheriBSD, including:

- DTrace

Many other kernel features are not yet well validated, including:

- Non-UFS file systems
- Most optional modules, such as various firewall systems

## Alpha ZFS support

The 22.12 release includes Alpha support for the ZFS file system.  It is
lightly tested, but works without known issues with a hybrid kernel.  With
the pure capability kernel, filesystems can be created and used, but it must
not be the root file system as it currently hangs on boot.

## Reporting bugs
Bug reports relating to these under-validated subsystems would be appreciated,
and may be submitted to the [CheriBSD GitHub issue
tracker](https://github.com/CTSRD-CHERI/cheribsd/issues).
