# Differences between CheriBSD and FreeBSD

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
- Two experimental OS compartmentalization models:
  - Colocated processes (co-processes) compartmentalization model (on a
    development branch)
  - Dynamic-linker-based compartmentalization model (on a development branch)
- Experimental CHERI-extended bhyve hypervisor (on a development branch) (on
  Morello only)

## Unsupported FreeBSD features

Certain FreeBSD features are currently unavailable in CheriBSD, including:

- ZFS
- DTrace
