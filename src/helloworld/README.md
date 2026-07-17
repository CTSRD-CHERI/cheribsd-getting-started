# Compiling "Hello World"

These instructions are intended for use on an Arm Morello board, and install
hybrid ABI versions of key toolchain and utilities using `pkg64`.
The commands will (eventually) work the same on CHERI-RISC-V, but output
details and registers in GDB will differ.

## Toolchain installation

You will need the Morello LLVM toolchain and Morello GDB for this
exercise. To install LLVM, use the command:

```
pkg64 install llvm-base
```

If this is the first time you are using `pkg64` on this system, you will
be prompted to bootstrap the `pkg` package before the package database
is downloaded and you are prompted to confirm before installing `llvm`
and its dependencies:

```
root@cheribsd:~ # pkg64 install llvm-base
The package management tool is not yet installed on your system.
Do you want to fetch and install it now? [y/N]: y
Bootstrapping pkg from http://pkg.CheriBSD.org/CheriBSD:20260710:aarch64, please wait...
Verifying signature with trusted certificate pkg.cheribsd.org.2022032901... done
Installing pkg-1.20.5_1...
Extracting pkg-1.20.5_1: 100%
Updating CheriBSD repository catalogue...
Fetching meta.conf: 100%    179 B   0.2kB/s    00:01
Fetching packagesite.pkg: 100%    7 MiB   7.2MB/s    00:01
Processing entries: 100%
CheriBSD repository update completed. 26229 packages processed.
All repositories are up to date.
Updating database digests format: 100%
The following 3 package(s) will be affected (of 0 checked):

New packages to be INSTALLED:
        llvm: 17,1
        llvm-base: 20240315
        llvm-morello: 17.0.d20260205_4

Number of packages to be installed: 3

The process will require 818 MiB more space.
116 MiB to be downloaded.

Proceed with this action? [y/N]: y
[1/3] Fetching llvm-base-20240315.pkg: 100%    1 KiB   1.1kB/s    00:01
[2/3] Fetching llvm-morello-17.0.d20260205_4.pkg: 100%  116 MiB  60.8MB/s    00:02
[3/3] Fetching llvm-17,1.pkg: 100%   12 KiB  11.8kB/s    00:01
Checking integrity... done (0 conflicting)
[1/3] Installing llvm-morello-17.0.d20260205_4...
[1/3] Extracting llvm-morello-17.0.d20260205_4: 100%
[2/3] Installing llvm-17,1...
[2/3] Extracting llvm-17,1: 100%
[3/3] Installing llvm-base-20240315...
[3/3] Extracting llvm-base-20240315: 100%
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

## Building for CheriABI

To build a CheriABI Hello World program use:

```
cc -g -O2 -Wall -o helloworld helloworld.c
```

You can verify this is a CheriABI binary with the `file` command:

```
user@cheribsd:~ $ file helloworld
helloworld: ELF 64-bit LSB pie executable, ARM aarch64, C64, CheriABI, version 1 (SYSV), dynamically linked, interpreter /libexec/ld-elf.so.1, for FreeBSD 15.0 (1500040), FreeBSD-style, with debug_info, not stripped
```

## Building for the Benchmark ABI

To target the [Benchmark ABI](../benchmarking/), add the argument
`-mabi=purecap-benchmark` to the `cc` command line:

```
cc -g -O2 -Wall -mabi=purecap-benchmark -o helloworld helloworld.c
```

You can verify this is a Benchmark ABI binary with the `file` command:
```
user@cheribsd:~ $ file helloworld
helloworld: ELF 64-bit LSB pie executable, ARM aarch64, C64, CheriABI, version 1 (SYSV), dynamically linked, interpreter /libexec/ld-elf.so.1, for FreeBSD 15.0 (1500040), FreeBSD-style, pure-capability benchmark ABI, with debug_info, not stripped
```

## Running

Run the program:

```
user@cheribsd:~ $ ./helloworld
Hello world
```

## Debugging

First, if it is not already installed, install the CHERI GDB debugger:

```
pkg64 install gdb-cheri
```

You can then debug `helloworld` by running GDB and setting a breakpoint
on `main` (note the output here is for the CheriABI helloworld binary, not the
Benchmark ABI one):

```
user@cheribsd:~ $ gdb ./helloworld
GNU gdb (GDB) 14.1 [GDB v14.1.d20260511 for FreeBSD]
...
Reading symbols from ./helloworld...
(gdb) b main
Breakpoint 1 at 0x107fc: file helloworld.c, line 6.
(gdb) r
Starting program: /home/user/helloworld

Breakpoint 1, main () at helloworld.c:6
6               printf("Hello world\n");
(gdb) 
```

If you then step into the `printf` (or `puts` depending on
optimization level) call you will see that GDB prints the capability
argument with expanded information including bounds and permissions:

```
(gdb) s
puts (s=0x100651 [rR,0x100651-0x10065d] "Hello world")
    at /local/scratch/jenkins/workspace/CheriBSD-pipeline_releng_26.07/cheribsd/lib/libc/stdio/puts.c:54
warning: 54     /local/scratch/jenkins/workspace/CheriBSD-pipeline_releng_26.07/cheribsd/lib/libc/stdio/puts.c: No such file or directory
```

This information is also included in backtraces:

```
(gdb) bt
#0  puts (s=0x100651 [rR,0x100651-0x10065d] "Hello world")
    at /local/scratch/jenkins/workspace/CheriBSD-pipeline_releng_26.07/cheribsd/lib/libc/stdio/puts.c:54
#1  0x0000000000110808 in main () at helloworld.c:6
```

The argument can also be examined directly as either an integer or capability
register:

```
(gdb) info reg x0
x0             0x100651            1050193
(gdb) info reg c0
c0             0x905c4000465d06510000000000100651 0x100651 [rR,0x100651-0x10065d]
```

If a capability (either in memory or a register) contains a
null-derived value, it is displayed as a simple scalar value without
any attributes.  If a capability is not null-derived but is untagged,
the string "(invalid)" is displayed after the bounds.

Capability tags in memory can also be examined by passing the "m"
flag to the eXamine command.  For example, compare the output of the
internal FILE structure used for stdout in libc:

```
(gdb) p *__stdoutp
$1 = {_p = 0x0, _r = 0, _w = 0, _flags = 8, _file = 1, _bf = {_base = 0x0,
    _size = 0}, _lbfsize = 0,
  _cookie = 0x403ec7f0 [rwRW,0x403ec620-0x403ecb90],
  _close = 0x402dece1 <__sclose> [rxRE,0x401f1200-0x403c7000] (sentry),
  _read = 0x402deca1 <__sread> [rxRE,0x401f1200-0x403c7000] (sentry),
  _seek = 0x402decd9 <__sseek> [rxRE,0x401f1200-0x403c7000] (sentry),
  _write = 0x402decbd <__swrite> [rxRE,0x401f1200-0x403c7000] (sentry), _ub = {
    _base = 0x0, _size = 0}, _up = 0x0, _ur = 0, _ubuf = "\000\000",
  _nbuf = "", _lb = {_base = 0x0, _size = 0}, _blksize = 0, _offset = 0,
  _fl_mutex = 0x0, _fl_owner = 0x0, _fl_count = 0, _orientation = 0,
  _mbstate = {__mbstate8 = '\000' <repeats 127 times>, _mbstateL = 0,
    _mbstateP = 0x0 }, _flags2 = 0}
(gdb) x/16gxm __stdoutp
<CHERI Tag 0 for range [0x403ec7f0,0x403ec800)>
0x403ec7f0:       0x0000000000000000      0x0000000000000000
<CHERI Tag 0 for range [0x403ec800,0x403ec810)>
0x403ec800:       0x0000000000000000      0x0000000000010008
<CHERI Tag 0 for range [0x403ec810,0x403ec820)>
0x403ec810:       0x0000000000000000      0x0000000000000000
<CHERI Tag 0 for range [0x403ec820,0x403ec830)>
0x403ec820:       0x0000000000000000      0x0000000000000000
<CHERI Tag 0 for range [0x403ec830,0x403ec840)>
0x403ec830:       0x0000000000000000      0x0000000000000000
<CHERI Tag 1 for range [0x403ec840,0x403ec850)>
0x403ec840:       0x00000000403ec7f0      0xdc5d40004b90c620
<CHERI Tag 1 for range [0x403ec850,0x403ec860)>
0x403ec850:       0x00000000402dece1      0xb05dc000b1c77c49
<CHERI Tag 1 for range [0x403ec860,0x403ec870)>
0x403ec860:       0x00000000402deca1      0xb05dc000b1c77c49
```

Walking up the stack frames we can see the different parts of the thread's
stack in use:

```
(gdb) info reg csp
csp            0xdc5c40003ffdbfff0000fffffff7fe40 0xfffffff7fe40 [rwRW,0xffffbff80000-0xfffffff80000]
(gdb) f 1
#1  0x0000000000110808 in main () at helloworld.c:6
6               printf("Hello world\n");
(gdb) info reg csp
csp            0xdc5c40003ffdbfff0000fffffff7ff60 0xfffffff7ff60 [rwRW,0xffffbff80000-0xfffffff80000]
```

## Debugging with compartmentalization

By default, CheriBSD runs without linkage-based compartmentalization enabled.
For more information on this technology, and the various ways it can be enabled
or disabled, see [Userlevel software compartmentalization](../features/c18n.md)
this guide.
First, enable compartmentalization for this binary:

```
user@cheribsd:~ $ elfctl -e +cheric18n helloworld
```

Then verify it runs as before:

```
user@cheribsd:~ $ ./helloworld
Hello world
```

Now, run it under GDB again and this time set a breakpoint on `puts` directly
(or `printf`, depending on what function you stepped into before):

```
user@cheribsd:~ $ gdb ./helloworld
GNU gdb (GDB) 14.1 [GDB v14.1.d20260511 for FreeBSD]
...
Reading symbols from ./helloworld...
(gdb) b puts
Function "puts" not defined.
Make breakpoint pending on future shared library load? (y or [n]) y
Breakpoint 1 (puts) pending.
(gdb) r
Starting program: /home/user/helloworld

Breakpoint 1, puts (s=0x100651 [rR,0x100651-0x10065d] "Hello world")
    at /local/scratch/jenkins/workspace/CheriBSD-pipeline_releng_26.07/cheribsd/lib/libc/stdio/puts.c:54
warning: 54     /local/scratch/jenkins/workspace/CheriBSD-pipeline_releng_26.07/cheribsd/lib/libc/stdio/puts.c: No such file or directory
(gdb) 
```

This time, if we look at a backtrace, there is an extra `<cross-compartment
call>` frame, since the call from `main` to `puts` is between two different
compartments, requiring a domain transition:

```
(gdb) bt
#0  puts (s=0x100651 [rR,0x100651-0x10065d] "Hello world")
    at /local/scratch/jenkins/workspace/CheriBSD-pipeline_releng_26.07/cheribsd/lib/libc/stdio/puts.c:54
#1  <cross-compartment call>, from "/helloworld" (ID: 3) to "[TCB]" (ID: 1)
#2  main () at helloworld.c:7
```

The `info compartments` command will list all compartments present in the
userspace process:

```
(gdb) info compartments
  Id Name        Libraries
   0 [RTLD]      /libexec/ld-elf.so.1
   1 [TCB]       libc.so.7 libthr.so.3 libsys.so.7
   2 [libunwind] libgcc_s.so.1
   3 /helloworld /helloworld
```

We can still inspect the argument register as before, which is unchanged:

```
(gdb) info reg x0
x0             0x100651            1050193
(gdb) info reg c0
c0             0x905c4000465d06510000000000100651 0x100651 [rR,0x100651-0x10065d]
```

If we look at `__stdoutp`, the various `sentry` capabilities present before no
longer point to the corresponding symbols (e.g. `_close` no longer points to
`__sclose`), since these function pointers are automatically wrapped by
trampolines to ensure any cross-compartment calls undergo a domain transition:

```
(gdb) p *__stdoutp
$1 = {_p = 0x0, _r = 0, _w = 0, _flags = 8, _file = 1, _bf = {_base = 0x0,
    _size = 0}, _lbfsize = 0,
  _cookie = 0x403ed7f0 [rwRW,0x403ed620-0x403edb90],
  _close = 0x408e752d [rxRE,0x408e7500-0x408e7668] (sentry),
  _read = 0x408e780d [rxRE,0x408e77e0-0x408e7948] (sentry),
  _seek = 0x408e7aed [rxRE,0x408e7ac0-0x408e7c28] (sentry),
  _write = 0x408e797d [rxRE,0x408e7950-0x408e7ab8] (sentry), _ub = {
    _base = 0x0, _size = 0}, _up = 0x0, _ur = 0, _ubuf = "\000\000",
  _nbuf = "", _lb = {_base = 0x0, _size = 0}, _blksize = 0, _offset = 0,
  _fl_mutex = 0x0, _fl_owner = 0x0, _fl_count = 0, _orientation = 0,
  _mbstate = {__mbstate8 = '\000' <repeats 127 times>, _mbstateL = 0,
    _mbstateP = 0x0 }, _flags2 = 0}
(gdb) x/16gxm __stdoutp
<CHERI Tag 0 for range [0x403ed7f0,0x403ed800)>
0x403ed7f0:       0x0000000000000000      0x0000000000000000
<CHERI Tag 0 for range [0x403ed800,0x403ed810)>
0x403ed800:       0x0000000000000000      0x0000000000010008
<CHERI Tag 0 for range [0x403ed810,0x403ed820)>
0x403ed810:       0x0000000000000000      0x0000000000000000
<CHERI Tag 0 for range [0x403ed820,0x403ed830)>
0x403ed820:       0x0000000000000000      0x0000000000000000
<CHERI Tag 0 for range [0x403ed830,0x403ed840)>
0x403ed830:       0x0000000000000000      0x0000000000000000
<CHERI Tag 1 for range [0x403ed840,0x403ed850)>
0x403ed840:       0x00000000403ed7f0      0xdc5d40005b90d620
<CHERI Tag 1 for range [0x403ed850,0x403ed860)>
0x403ed850:       0x00000000408e752d      0xb05fc000f6687500
<CHERI Tag 1 for range [0x403ed860,0x403ed870)>
0x403ed860:       0x00000000408e780d      0xb05fc000f94877e0
```

We can also see that different stacks are being used between the different
compartments, not just parts of the same single stack:

```
(gdb) info reg csp
csp            0xdc5c40003456b4570000000040f44e80 0x40f44e80 [rwRW,0x40b45000-0x40f45000]
(gdb) f 2
#2  main () at helloworld.c:7
7       }
(gdb) info reg csp
csp            0xdc5e40002006a0070000000041dfffc0 0x41dfffc0 [rwRW,0x41a00000-0x41e00000]
```
