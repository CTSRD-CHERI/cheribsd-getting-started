# Building image files

CheriBSD can be built natively on an existing CheriBSD system (e.g., on an Arm
Morello box), and it can also be cross-built from FreeBSD, Linux, and macOS.
Building natively from an ISA-level emulator, such as QEMU, is not recommended
for performance reasons.

- Native builds follow the [same build
  procedure](https://docs.freebsd.org/en/books/handbook/cutting-edge/#makeworld)
  as used for the baseline FreeBSD operating system.
  Suitable git command-line substitions must be made to use CheriBSD rather
  than FreeBSD source code.

- CheriBSD cross-build orchestration is performed by the Python-based
  [cheribuild](https://github.com/CTSRD-CHERI/cheribuild) tool, which is
  able to build CHERI-enabled toolchain, operating-system stack, and various
  applications and other software used by the CHERI project.

## Key source repositories

CheriBSD repositories currently reside in the [CTSRD-CHERI GitHub
Organization](https://github.com/CTSRD-CHERI):

* [cheribuild GitHub repository](https://github.com/CTSRD-CHERI/cheribuild)
* [CheriBSD GitHub repository](https://github.com/CTSRD-CHERI/cheribsd)
* [CHERI Clang/LLVM GitHub repository](https://github.com/CTSRD-CHERI/llvm-project)
* [CHERI GDB GitHub repository](https://github.com/CTSRD-CHERI/gdb)
* [QEMU-CHERI GitHub repository](https://github.com/CTSRD-CHERI/qemu)

Unless you intend to modify CheriBSD, CHERI Clang/LLVM, CHERI GDB, or
QEMU-CHERI, you should not generally need to manually check out or compile
most of the above repositories.
Instead, existing prebuilt software images/packages should be used, or the
cheribuild command, which will orchestrate software cross-build for you.

## Checking out cheribuild

[cheribuild](https://github.com/CTSRD-CHERI/cheribuild) is a Python-based
build orchestration tool that is the preferred way to cross-build CheriBSD.
It can be checked out from GitHub:

```
git clone git@github.com:CTSRD-CHERI/cheribuild.git
```

## Building and running CheriBSD/RISC-V in QEMU

The following command builds a CheriBSD/RISC-V live image, and boots it in
QEMU-CHERI:

```
% ./cheribuild.py -d build-and-run-cheribsd-riscv64-purecap
```

## Building and running CheriBSD/Morello in QEMU

The following command builds a CheriBSD/Morello live image, and boots it in
QEMU-CHERI:

```
% ./cheribuild.py -d build-and-run-cheribsd-morello-purecap
```

## Building CheriBSD/Morello release images

The following command builds a CheriBSD/Morello installer (memstick) image
suitable to write to a USB stick:

```
% ./cheribuild.py -d --clean cheribsd-release-morello-purecap
```

The resulting image file will be generated in a file with a name along the
lines of:
`output/cheribsd-release-morello-purecap/FreeBSD-15.0-CURRENT-arm64-aarch64c-memstick.img`.
This is relative to your `cheribuild` destination root, which is, by default,
`~/cheri`.

## Obtaining further information

The `cheribuild` [README.md](https://github.com/CTSRD-CHERI/cheribuild#readme)
provides detailed information on various parameters and targets its supports,
as well as its dependencies.
