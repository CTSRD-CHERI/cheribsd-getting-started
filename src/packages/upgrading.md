# Upgrading packages

CheriBSD does not currently provide base system upgrades.
However, package versions might change not only between CheriBSD versions but
also during a lifetime of a single CheriBSD version, if a bug was found in
a package or a new package was adapted to CHERI platforms allowing more
third-party software to be available for CheriBSD.
The following sections describe how CheriBSD packages are associated with
particular CheriBSD versions and how to upgrade them once new packages are
published.

## Package ABI version

Packages are built for a specific ABI version and an architecture of CheriBSD,
not for a specific CheriBSD version.
In case there are no ABI differences between two CheriBSD versions, the same
package set should work for both of them.

You can check a current ABI version for your packages with:

| CheriABI            | Hybrid ABI        |
|---------------------|-------------------|
| `pkg64c config ABI` | `pkg64 config ABI` |

## Package upgrades

A new version of a package can be published in the following cases:
* A bug reported by a user was fixed in the package;
* A package had been broken or missing and was adapted to CheriBSD;
* A dependency of the package had been broken or missing and was adapted to
CheriBSD which enabled the package to be built for CheriBSD.

There are currently no plans to publish announcements on package upgrades.
It is advised to check available package versions with commands described
further in this section as well as contact CheriBSD developers and other users
via the [CHERI CPU Slack](../support/) or the
[CheriBSD ports mailing list](../support/)
to discuss new package versions.

You can review if there is a new package version and upgrade it after
confirmation with:

| CheriABI                       | Hybrid ABI                    |
|--------------------------------|-------------------------------|
| `pkg64c upgrade <pkg-name>` | `pkg64 upgrade <pkg-name>` |

Similarly, you can review if any of your packages has a new version and 
upgrade all packages after confirmation with:

| CheriABI                       | Hybrid ABI                    |
|--------------------------------|-------------------------------|
| `pkg64c upgrade` | `pkg64 upgrade` |

If you want to only review new package versions without upgrading them, add
the `-n` flag to your `upgrade` command. You can read more on the `upgrade`
command at
[pkg-upgrade(8)](https://www.freebsd.org/cgi/man.cgi?pkg-upgrade(8))
or in your console with:
```
man pkg-upgrade
```
