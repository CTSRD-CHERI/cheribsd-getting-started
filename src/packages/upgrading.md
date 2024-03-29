# Upgrading packages

It is recommended to upgrade packages in the following situations:

* Before building a newer CheriBSD release;

  The release might require an updated toolchain (e.g., due to ABI changes in
  the release that must be supported by the toolchain).

* After updating your host to a newer CheriBSD release;

  Package repositories are compiled for the ABI version used by the
  corresponding release, which might differ from the ABI of the previous
  release.

  Where practical, we intend to support packages from the immediately prior
  release on the newer release, but will make no effort to ensure that package
  repositories from even older releases continue to work.

  Additionally, package repositories for the newer release will most likely
  include updated third-party software versions that are tested against that
  release.

* After finding a bug in a package (e.g., a library, a toolchain).

  It might happen that the bug you discovered has already been reported and
  fixed.
  If the package upgrade does not resolve your issue, please
  [report the bug](../support/).

Use the following commands to upgrade your packages:

| CheriABI         | Hybrid ABI      |
|------------------|-----------------|
| `pkg64c upgrade` | `pkg64 upgrade` |

If you do not want perform the upgrade itself but only check if newer versions
are available, use the `-n` flag:

| CheriABI            | Hybrid ABI         |
|---------------------|--------------------|
| `pkg64c upgrade -n` | `pkg64 upgrade -n` |

You can read more on the `upgrade` command at
[pkg-upgrade(8)](https://www.freebsd.org/cgi/man.cgi?pkg-upgrade(8))
or in your console with `man pkg-upgrade`.
