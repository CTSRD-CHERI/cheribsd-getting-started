# CheriBSD features

CheriBSD extends FreeBSD with added CHERI-enabled features, such as kernel and
userspace memory safety:

 * [Kernel compilation modes](kernel.md)
 * [Process environments](processes.md)
 * [CheriABI desktop environment](desktop.md)
 * [Userlevel heap temporal memory safety (experimental - enabled by
default)](temporal.md)
 * [Userlevel software compartmentalization (experimental - disabled by
default)](c18n.md)
 * [bhyve hypervisor (experimental)](bhyve.md)

All of these features ship in the CheriBSD release, but some are not
enabled by default.
