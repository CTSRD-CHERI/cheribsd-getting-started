# Limitations of packages

When using or even considering third-party software delivered with CheriBSD
packages, it is important to keep in mind the maturity of currently available
CheriABI packages and potential conflicts between hybrid ABI and CheriABI
packages.
The following sections describe some of such limitations.

## Maturity of CheriABI packages

There are many projects that must still be ported to CHERI in order to provide
a fully functional CheriABI package repository.
The available CheriABI packages are also not continuously tested as part of the
CheriBSD package building infrastructure.

CheriABI packages compile, but many have CHERI-related warnings that
have not been audited and only a limited set have been tested.
While the CheriABI packages are more interesting as they use CHERI memory-safety
features, you must remember that they might break in run-time and hence might
not be suitable for critical operations.
In such case, a corresponding hybrid ABI package might be considered instead of
a CheriABI package.

We expect the number of available and tested packages to grow with future
CheriBSD releases but it will take quite some time until we will be able to
provide a CheriABI package repository that would include all packages currently
provided with the hybrid ABI package repository.

We encourage everyone to report bugs related to CHERI support in the
[CheriBSD ports issue tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).

## Separate local bases for ABIs

Upstream FreeBSD does not have any notion of an ABI at the package manager
level and installs all packages in the default local base path (`/usr/local`).
CheriBSD introduces a workaround for this issue by using separate local base
paths for different package repositories.

The hybrid ABI packages are compiled with a local base set to `/usr/local64`
while the CheriABI packages use the default local base.
Because the hybrid ABI packages are installed in a non-standard location, there
may be bugs resulting in hybrid ABI packages looking for its dependencies in
`/usr/local` instead of `/usr/local64`.
For example, a CheriBSD port a package was built from might not configure
a project-specific build system to use a custom local base or an upstream
project might hardcode `/usr/local` in its source code.

We encourage everyone to report bugs related to invalid local base paths in the
hybrid ABI packages in
[CheriBSD ports issue tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).

## Cross-ABI package conflicts

Hybrid ABI and CheriABI package repositories include many counterpart packages,
e.g. `git` is available in both repositories.
CheriABI packages have a higher priority than the hybrid ABI packages in default
`PATH` environment variables in CheriBSD.
In case you installed a CheriABI package and a hybrid ABI package with a
conflicting program name, you must execute the hybrid ABI program using
an absolute path.

When using a shell configuration not included in CheriBSD's source code, a user
must remember to appropriately set the `PATH` environment variable to take into
account both `/usr/local` and `/usr/local64` paths.
It is also recommended to place `/usr/local` before `/usr/local64` in such
configuration to match the CheriBSD's approach.

## Cross-ABI package dependencies

At the moment, a package cannot depend in run-time on another package with
a different ABI, e.g. to execute a program provided by that package.
Such feature would be useful if a CheriBSD port cannot easily be adapted to
CheriABI and includes a program that is executed by another port that has
a CheriABI package.
There are currently no plans to support this case.
