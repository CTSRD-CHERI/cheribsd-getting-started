# Differences between CheriBSD and FreeBSD

## New CHERI-based features

CheriBSD extends FreeBSD with the following CHERI-based features:

- Pure-capability spatially safe kernel
- Hybrid kernel
- Spatially safe userspace ("CheriABI")
- Hybrid userspace
- CheriABI third-party packages (experimental)
- Hybrid third-party packages
- Experimental temporally safe userspace (on a development branch)
- Experimental colocated processes (co-processes) compartmentalization model
  (on a development branch)
- Experimental dynamic-linker-based compartmentalization model (on a
  development branch)
- Experimental CHERI-extended bhyve hypervisor (on a development branch) (on
  Morello only)

## Unsupportable FreeBSD features

Certain FreeBSD features are currently unavailable in CheriBSD, including:

- ZFS
- DTrace
