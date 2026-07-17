# Unsupported FreeBSD features

Certain FreeBSD features are currently unavailable in CheriBSD, including:

- DTrace on hybrid kernels or with the kinst provider
- Linux binary compatibility ("Linuxulator")

Many other kernel features are not yet well validated, including:

- Most optional modules, such as various firewall systems
- Some advanced ZFS features; see the release notes for caveats before
  deviating from the default configuration used in this guide.

## Reporting bugs
Bug reports relating to these under-validated subsystems would be appreciated,
and may be submitted to the [CheriBSD GitHub issue
tracker](https://github.com/CTSRD-CHERI/cheribsd/issues).
