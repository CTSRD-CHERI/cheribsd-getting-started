# CheriABI "Hello World"

These instructions are intended for use on an Arm Morello board, and install
hybrid ABI versions of key toolchain and utilities using `pkg64`.
The commands will (eventually) work the same on CHERI-RISC-V, but output
details and registers in GDB will differ.

## Toolchain installation

You will need the Morello LLVM toolchain and Morello GDB for this
exercise. To install LLVM, use the command:

```pkg64 install llvm-base```

If this is the first time you are using `pkg64` on this system, you will
be prompted to bootstrap the `pkg` package before the package database
is downloaded and you are prompted to confirm before installing `llvm`
and its dependencies:

```
root@cheribsd:~ # pkg64 install llvm-base
The package management tool is not yet installed on your system.
Do you want to fetch and install it now? [y/N]: y
Bootstrapping pkg from pkg+http://pkg.CheriBSD.org/CheriBSD:20220314:aarch64, please wait...
Verifying signature with trusted certificate pkg.cheribsd.org.2022032901... done
Installing pkg-1.17.5_1...
Extracting pkg-1.17.5_1: 100%
Updating CheriBSD repository catalogue...
Fetching meta.conf: 100%    163 B   0.2kB/s    00:01
Fetching packagesite.pkg: 100%    4 MiB   1.4MB/s    00:03
Processing entries: 100%
CheriBSD repository update completed. 20954 packages processed.
All repositories are up to date.
Updating database digests format: 100%
The following 12 package(s) will be affected (of 0 checked):

New packages to be INSTALLED:
        gettext-runtime: 0.21
        indexinfo: 0.3.1
        libedit: 3.1.20210910,1
        libffi: 3.3_1
        libxml2: 2.9.13_2
        llvm: 13,1
        llvm-base: 1
        llvm-morello: 13.0.d20220502_1
        mpdecimal: 2.5.1
        perl5: 5.32.1_1
        python38: 3.8.13
        readline: 8.1.2

Number of packages to be installed: 12

The process will require 902 MiB more space.
190 MiB to be downloaded.

Proceed with this action? [y/N]: y
[1/12] Fetching llvm-base-1.pkg: 100%    3 KiB   2.8kB/s    00:01
[2/12] Fetching llvm-13,1.pkg: 100%   11 KiB  11.2kB/s    00:01
...
[12/12] Fetching libedit-3.1.20210910,1.pkg: 100%  119 KiB 121.8kB/s    00:01
Checking integrity... done (0 conflicting)
[1/12] Installing indexinfo-0.3.1...
[1/12] Extracting indexinfo-0.3.1: 100%
...
[12/12] Installing llvm-base-1...
[12/12] Extracting llvm-base-1: 100%
=====
...

```

*Note:* By default FreeBSD ships with the `vi` and `ee` editors. You may
wish to install the `nano` or `vim` package to access a
more familiar editor. Currently only hybrid packages installable with
`pkg64` are available.

## Source code

```C
#include <stdio.h>

int
main(void)
{
	printf("Hello world\n");
}
```

## Building

To build a CheriABI Hello World program use:

```cc -g -O2 -Wall -o helloworld helloworld.c```

You can verify this is a CheriABI binary with the `file` command:

```
root@cheribsd:~ # file helloworld
helloworld: ELF 64-bit LSB pie executable, ARM aarch64, C64, CheriABI, version 1 (SYSV), dynamically linked, interpreter /libexec/ld-elf.so.1, for FreeBSD 14.0 (1400064), FreeBSD-style, with debug_info, not stripped
```

## Running

Run the program:

```
root@cheribsd:~ # ./helloworld
Hello world
```

## Debugging

First, if it is not already installed, install the CHERI GDB debugger:

```pkg64 install gdb-cheri```

You can then debug `helloworld` by running GDB and setting a breakpoint
on `main`:

```
root@cheribsd:~ # gdb ./helloworld
GNU gdb (GDB) 12.1
...
Reading symbols from ./helloworld...
...
(gdb) b main
Breakpoint 1 at 0x10a14: file helloworld.c, line 6.
(gdb) r
Starting program: /root/helloworld

Breakpoint 1, main () at helloworld.c:6
6           printf("Hello world\n");
(gdb) 
```

If you then step into the `printf` (or `puts` depending on
optimization level) call you will see that GDB prints the capability
argument with expanded information including bounds and permissions:

```
(gdb) s
puts (s=0x1006e0 [rR,0x1006e0-0x1006ec] "Hello world")
    at /home/bed22/cheri/cheribsd/lib/libc/stdio/puts.c:60
60      /home/bed22/cheri/cheribsd/lib/libc/stdio/puts.c: No such file or directory.
```
The argument can also be examined directly as either an integer or capability register:
```
(gdb) info reg x0
x0             0x1006e0            1050336
(gdb) info reg c0
c0             0x905f400046ec06e000000000001006e0 0x1006e0 [rR,0x1006e0-0x1006ec]
(gdb)
