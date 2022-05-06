<!-- ANCHOR: cover -->

# Getting Started with CheriBSD

Robert N. M. Watson (University of Cambridge), Brooks Davis (SRI International), ...

This repository contains a living document describing how to get up and
running with [CheriBSD](https://www.cheribsd.org/) on Morello and
CHERI-RISC-V.

## Acknowledgements

The authors gratefully acknowledge Jessica Clarke, George Neville-Neil, ...

*This document is a work-in-progress.  Feedback and contributions are
welcomed.  Please see our [GitHub
Repository](https://github.com/CTSRD-CHERI/cheribsd-getting-started) for the
source code and an issue tracker.*

This material is based upon work supported by the Defense Advanced
Research Projects Agency (DARPA) under Contract No. HR001122C0110 ("ETC"). Any
opinions, findings and conclusions or recommendations expressed in this
material are those of the author(s) and do not necessarily reflect the
views of the Defense Advanced Research Projects Agency (DARPA).

<!-- ANCHOR_END: cover -->

## Building

Building the book from the Markdown sources requires
[mdBook](https://github.com/rust-lang/mdBook). Once installed, `mdbook build`
will build the static HTML files in the `book/` directory, whilst `mdbook
serve` will build and serve them at `http://localhost:3000`. Please refer to
the mdBook documentation for futher options.
