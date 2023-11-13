# Debugging packages

Similarly to the FreeBSD Project, CheriBSD packages are built with CheriBSD
ports that are based on FreeBSD ports.
By default, the packages are built with optimisation levels defined in CheriBSD
ports or upstream third-party software projects.
Unfortunately, the FreeBSD ports build system does not allow to build ports with
optimisations and debug symbols at the same time.
As a consequence of that, you might discover that debug symbols are missing when
debugging a program with the GDB debugger.

In order to allow users to use optimised and debug CheriABI packages, CheriBSD
provides two package repositories with them: the default repository with
optimisations and without debug symbols called `CheriBSD`, and the debug
repository without optimisations and with debug symbols called `CheriBSD-debug`.
Note that we do not provide debug packages for the hybrid and benchmark ABIs.

You can enable the debug repository by first creating a directory for custom
package repository configuration files:
```
mkdir -p /usr/local/etc/pkg/repos
```
and then creating a configuration file that enables `CheriBSD-debug`:
```
echo "CheriBSD-debug: { enabled: yes }" >/usr/local/etc/pkg/repos/CheriBSD-debug.conf
```

You can reinstall all previously installed default packages to replace them with
debug packages:

```
pkg64c upgrade -fr CheriBSD-debug
```

Alternatively, you can selectively reinstall a package:

```
pkg64c upgrade -r CheriBSD-debug <pkg-name>
```

However, keep in mind that in such case only the package you list is
reinstalled, leaving its dependencies intact.

Once you enable the `CheriBSD-debug` repository, the `CheriBSD` repository is
used by default.
You have to use the `-r` flag to switch to `CheriBSD-debug` in a `pkg64c`
command, e.g.:

```
pkg64c install -r CheriBSD-debug <pkg-name>
```

If you want to use only the `CheriBSD-debug` repository, you can create a
configuration file that disables the `CheriBSD` repository:

```
echo "CheriBSD: { enabled: no }" > /usr/local/etc/pkg/repos/CheriBSD.conf
```

You can also check if an installed package was fetched from the default or debug
package repository:

```
pkg64c query '%n %R' <pkg-name>
```

You can omit `<pkg-name>` in the above command to list all packages with their
repositories.
