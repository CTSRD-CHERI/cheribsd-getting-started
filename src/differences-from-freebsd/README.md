# Differences between CheriBSD and FreeBSD

CheriBSD extends FreeBSD with added CHERI-enabled features, such as kernel and
userspace memory safety.
Some of these features are in the main CheriBSD branch; other experimental
features  remain on development branches.
Certain FreeBSD features are not yet available in CheriBSD.

## New CHERI-based features

CheriBSD extends FreeBSD with the following CHERI-based features:

- Two kernel compilation modes:
  - Pure-capability spatially safe kernel
  - Hybrid kernel
- Two userspace process environments:
  - Spatially safe userspace ("CheriABI")
  - Hybrid userspace
- Two sets of third-party packages
  - CheriABI packages (experimental)
  - Hybrid packages
- Experimental temporally safe userspace (on a development branch)
- Experimental OS compartmentalization models:
  - Colocated processes (co-processes) compartmentalization model (on a
    development branch)
  - Dynamic-linker-based compartmentalization model (on a development branch)
- Experimental CHERI-extended bhyve hypervisor (on a development branch) (on
  Morello only)

## Unsupported FreeBSD features

Certain FreeBSD features are currently unavailable in CheriBSD, including:

- ZFS
- DTrace
