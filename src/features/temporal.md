# Userlevel heap temporal memory safety (experimental)

CheriBSD implements, on an experimental development branch, support for the
Cornucopia heap temporal safety algorithm, as well as successor algorithms
based on load-side-barrier features present in the Morello prototype
architecture and processor design.
Cornucopia is described in an [IEEE SSP 2020
paper](https://www.cl.cam.ac.uk/research/security/ctsrd/pdfs/2020oakland-cornucopia.pdf).
