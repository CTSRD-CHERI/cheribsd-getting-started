# Limitations of packages

When using or even considering third-party software delivered with CheriBSD
packages, it is important to keep in mind the maturity of currently available
CheriABI packages, and potential conflicts between hybrid ABI and CheriABI
packages.
The following sections describe some of such limitations.

## Maturity of CheriABI packages

There are many projects that must still be ported to CHERI in order to provide
a fully-functional CheriABI package repository.
The available CheriABI packages are also not continuously tested as part of the
CheriBSD package building infrastructure.

CheriABI packages compile, but many have CHERI-related warnings that have not
been audited, and only a limited set have been tested.
Whilst the CheriABI packages are more interesting as they use CHERI
memory-safety features, they might break at run time and hence might not be
suitable for critical operations.
In such cases, a corresponding hybrid ABI package might be better suited than
the CheriABI package.

We expect the number of available and tested packages to grow with future
CheriBSD releases, but it will take quite some time until we are able to provide
a CheriABI package repository that covers functionalities currently included
in the hybrid ABI package repository.

We encourage everyone to report bugs related to CHERI support in the
[CheriBSD ports issue tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).

## Separate local bases for ABIs

Upstream FreeBSD does not have the ability to manage packages for multiple ABIs
at the same time and its `pkg` package manager installs all packages in the
default local base path (`/usr/local`).
CheriBSD introduces a workaround for this issue by using separate local base
paths for different package repositories.

The hybrid ABI packages are compiled with a local base set to `/usr/local64`
while the CheriABI packages use the default local base.
Because the hybrid ABI packages are installed in a non-standard location, there
may be bugs resulting in hybrid ABI packages looking for its dependencies in
`/usr/local` instead of `/usr/local64`.
For example, a CheriBSD port a package was built from might fail to configure
a project's build system to use this custom local base, or an upstream project
might hardcode `/usr/local` in its source code.

We encourage everyone to report bugs related to invalid local base paths in the
hybrid ABI packages in our
[CheriBSD ports issue tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).

## Cross-ABI package conflicts

Hybrid ABI and CheriABI package repositories include many counterpart packages,
e.g. `git` is available in both repositories.
CheriABI packages have a higher priority than the hybrid ABI packages in default
`PATH` environment variables in CheriBSD.
If you install a CheriABI package and a hybrid ABI package with a conflicting
program name, you must execute the hybrid ABI program using an absolute path by
default.

When using a shell configuration not included in CheriBSD's source code, a user
must remember to appropriately set the `PATH` environment variable to take into
account both `/usr/local` and `/usr/local64` paths.
The user can set the order of the paths they prefer but it is recommended to
place `/usr/local` before `/usr/local64` in such configurations to match
CheriBSD's defaults.

## Cross-ABI package dependencies

At the moment, a package cannot depend at run time on another package with
a different ABI, e.g. to execute a program provided by that package.
Such a feature would be useful for CheriBSD ports which cannot easily be adapted
to CheriABI and which include programs that could be executed by other ports
already adapted to CheriABI.
There are currently no plans to support this case.
