# Issues with packages

When using or even considering third-party software delivered with CheriBSD
packages, it is important to keep in mind potential conflicts between hybrid ABI
and CheriABI packages as well as limited access to software adapted to CHERI
platforms.
The following sections describe some of such issues.

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

## Missing packages

Hybrid ABI and CheriABI package repositories might be missing third-party
software due to the following reasons:
* There is no FreeBSD port including that software or it is broken on AArch64;

  Check [FreshPorts](https://www.freshports.org/),
  [FreeBSD Wiki](https://wiki.freebsd.org/WantedPorts),
  [FreeBSD Bugzilla](https://bugs.freebsd.org/bugzilla/) and
  the [freebsd-ports mailing
  list](https://lists.freebsd.org/subscription/freebsd-ports) to find out
  if anyone is working on software you are interested in.

* A CheriBSD port with the software is broken on Morello or is not adapted to
CheriABI.

  Check the [CheriBSD ports issue
  tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues) to find out if
  an issue with the software is documented and the [Poudriere infrastructure for
  CheriBSD packages](https://poudriere.cheribsd.org) to find failed build
  results.
