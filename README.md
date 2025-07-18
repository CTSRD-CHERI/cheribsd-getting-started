<!-- ANCHOR: cover -->

# Getting Started with CheriBSD 25.03

Robert N. M. Watson (University of Cambridge),
Jessica Clarke (University of Cambridge),
Brooks Davis (SRI International),
Konrad Witaszczyk (University of Cambridge)
and
George V. Neville-Neil (MSB Associates)

This is a living document that describes how to get up and running with
[CheriBSD](https://www.cheribsd.org/) on Morello and CHERI-RISC-V.
Topics include how to download and install prebuilt CheriBSD images, how to
build your own images, how third-party packages work, and where to find
further information and support.

<!--
NOTE: A release version is also in SUMMARY.md.
-->
**The document describes CheriBSD as of the 25.03 release, unless explicitly
stated in sections referring to earlier or later releases.**

*This document is a work-in-progress.  Feedback and contributions are
welcomed.  Please see our [GitHub
Repository](https://github.com/CTSRD-CHERI/cheribsd-getting-started) for the
source code and an issue tracker.
There is a [rendered version on the
web](https://ctsrd-cheri.github.io/cheribsd-getting-started/), which is
automatically updated when the git repository is committed to.*

## Acknowledgements

The authors gratefully acknowledge Brian Campbell (University of Edinburgh)
and Elise Haldane (Digital Catapult) for their contributions to this document.

This work was supported in part by the Innovate UK project Digital
Security by Design (DSbD) Technology Platform Prototype, 105694, and Innovate
UK project Developing and Evaluating an Open-Source Desktop for Arm Morello,
10027440.

This material is based upon work supported by the Defense Advanced
Research Projects Agency (DARPA) under Contract No. HR001122C0110 ("ETC"),
Defense Advanced Research Projects Agency (DARPA) Contract No. HR001123C0031
("MTSS"), and Defense Advanced Research Projects Agency (DARPA) Contract No.
FA8750-24-C-B047 ("DEC").
Any opinions, findings and conclusions or recommendations expressed in this
material are those of the author(s) and do not necessarily reflect the
views of the Defense Advanced Research Projects Agency (DARPA).

<!-- ANCHOR_END: cover -->

## Building

Building the book from the Markdown sources requires
[mdBook](https://github.com/rust-lang/mdBook). Once installed, `mdbook build`
will build the static HTML files in the `book/` directory, whilst `mdbook
serve` will build and serve them at `http://localhost:3000`. Please refer to
the mdBook documentation for futher options.
