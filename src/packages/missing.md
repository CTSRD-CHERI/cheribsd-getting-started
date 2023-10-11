# Missing packages

Hybrid ABI and CheriABI package repositories might be missing third-party
software due to the following reasons:
* There is no FreeBSD port including that software or it is broken on AArch64;

  Check [FreshPorts](https://www.freshports.org/),
  [FreeBSD Wiki](https://wiki.freebsd.org/WantedPorts),
  [FreeBSD Bugzilla](https://bugs.freebsd.org/bugzilla/) and
  the [freebsd-ports mailing
  list](https://lists.freebsd.org/subscription/freebsd-ports) to find out
  if anyone is working on software you are interested in.

* A CheriBSD port with the software is broken on Morello (e.g., it is not
  adapted to CheriABI), or does not respect a custom local base (`/usr/local64`)
  when compiled for the hybrid ABI.

  Check the [CheriBSD ports issue
  tracker](https://github.com/CTSRD-CHERI/cheribsd-ports/issues) to find out if
  an issue with the software is documented and the [Poudriere infrastructure for
  CheriBSD packages](https://poudriere.cheribsd.org) to find failed build logs
  and skipped build causes.
