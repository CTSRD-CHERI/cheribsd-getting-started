# Limitations of packages

When using or even considering third-party software delivered with CheriBSD
packages, it is important to keep in mind potential conflicts between hybrid ABI
and CheriABI packages as well as limited access to software adapted to CHERI
platforms.
The following sections describe some of such limitations.

## Maturity of CheriABI packages

*Note:* These packages compile, but many have CHERI-related warnings that
have not been audited and only a limited set have been tested. Bugs related to
CHERI support can be reported in the [CheriBSD ports issue
tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).
While the CheriABI packages are more interesting as they use CHERI memory-safety
features, you must remember that they might break in run-time and hence might
not be suitable for critical operations.
In such case, a corresponding hybrid ABI package might be considered instead of
a CheriABI package.

## Separate local bases for ABIs

*Note:* Because these packages are installed in a non-standard location,
there may be bugs related to them looking in `/usr/local` instead of
`/usr/local64` for dependencies. Please report bugs of this sort in
the [CheriBSD ports issue tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues).

## Cross-ABI package conflicts

Hybrid ABI and CheriABI package repositories include many counterpart packages,
e.g. `git` is available in both repositories.
If you decided to use a CheriABI package and find that it crashes, you
might consider using a hybrid ABI package instead.

CheriABI packages have a higher priority than the hybrid ABI packages in default
`PATH` environment variables in CheriBSD.
In case you installed a CheriABI package and a hybrid ABI package with a
conflicting program name, you must execute the hybrid ABI program using
an absolute path.

## Cross-ABI package dependencies

At the moment, a package cannot depend in run-time on another package with
a different ABI, e.g. to execute a program provided by that package.
Such feature would be useful if a CheriBSD port cannot easily be adapted to
CheriABI and includes a program that is executed by another port that has
a CheriABI package.
There are currently no plans to support this case.
