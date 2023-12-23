# Kernel compilation modes

The CheriBSD kernel can be compiled as hybrid, pure-capability,
or benchmark code:

- The **hybrid kernel** enables capability use in userspace while making
  relatively little use of capabilities in the kernel's own implementation.
- The **pure-capability kernel** implements strong referential and spatial
  memory protection internally in the kernel, protecting against memory-safety
  vulnerabilities in components such as the network stack and system-call
  layer.
- The **benchmark kernel** implements the [Benchmark ABI](../benchmarking/)
  which improves performance on the Arm Morello board at the cost of reduced
  security.
  The benchmark kernel is considered appropriate for a performance
  evaluation of the Arm Morello architecture and third-party software
  executed on it.
  It should not be used for a security evaluation and the
  pure-capability kernel should be used instead.

A number of precompiled kernels are provided in the CheriBSD release, enabling
or disabling debugging in combination with hybrid, pure-capability, and
benchmark compilations.
In general, we recommend that performance benchmarking be done only with
benchmark `NODEBUG` kernels;
see [Benchmarking guidance](../benchmarking/README.md) for further details.

## Precompiled kernels in CheriBSD

CheriBSD ships with a number of precompiled kernels.
Kernel configuration files themselves may be found in `sys/arm64/conf` in the
CheriBSD source tree, and will have names such as `GENERIC-MORELLO-PURECAP`.
Kernel configuration file names start with one of the following prefixes
identifying the compilation mode of the kernel:

| Prefix            | Description |
|-------------------|-------------|
| `GENERIC-MORELLO` | Compiled without internal memory safety |
| `GENERIC-MORELLO-PURECAP` | Compiled with internal memory safety |
| `GENERIC-MORELLO-PURECAP-BENCHMARK` | Compiled with the Benchmark ABI |

In addition to the base kernels for each compilation mode, CheriBSD includes
precompiled kernels which disable debugging.
These kernel configurations append a `-NODEBUG` suffix to the kernel
configuration file name.

The default kernel uses the `GENERIC-MORELLO-PURECAP` kernel
configuration, and is a pure-capability kernel with debugging enabled.
It is unsuitable for benchmarking; instead, use the
`GENERIC-MORELLO-PURECAP-BENCHMARK-NODEBUG` kernel configuration.

Kernels and their associated modules are stored in subdirectories of `/boot`
matching the pattern `kernel*`; the default kernel is named `/boot/kernel`.

```
# cd /boot
# ls -d kernel*
kernel
kernel.GENERIC-MORELLO
kernel.GENERIC-MORELLO-NODEBUG
kernel.GENERIC-MORELLO-PURECAP-BENCHMARK
kernel.GENERIC-MORELLO-PURECAP-BENCHMARK-NODEBUG
kernel.GENERIC-MORELLO-PURECAP-NODEBUG
```

## Identifying the currently booted kernel

The `uname(1)` command prints information on the kernel configuration.
The following command-line options may be of use:

| Option name | Description                         |
|-------------|-------------------------------------|
| `-a`        | Print numerous details              |
| `-i`        | Print the kernel configuration name |

For example:

```
# uname -i
GENERIC-MORELLO-PURECAP
```

Additionally, the current kernel file path is stored in the `kern.bootfile`
sysctl variable that you can read with the `sysctl(8)` utility.
For example:

```
# sysctl kern.bootfile
kern.bootfile: /boot/kernel/kernel
```

## Selecting another kernel using nextboot(8)

You can request an alternate kernel for the next boot of the system via
the `-k` flag of the `nextboot(8)` command.
The value of the flag should be set to the name of the kernel directory
under `/boot`; for example, to use a pure-capability kernel without
debugging on the next boot, use:

```
# nextboot -k kernel.GENERIC-MORELLO-PURECAP-NODEBUG
```

prior to rebooting.
The system will revert back to the kernel configured in `/boot/loader.conf`
on subsequent boots.

## Selecting another kernel using loader.conf

You can set the kernel to boot via `/boot/loader.conf` using the variable
`kernel`.
This is set to the name of the kernel directory under `/boot`; for example,
the default is:

```
kernel="kernel"
```

You can persistently set the system to boot with a Benchmark ABI kernel by
setting `kernel` as follows in `/boot/loader.conf`:

```
kernel="kernel.GENERIC-MORELLO-PURECAP-BENCHMARK"
```

After a reboot, `uname(1)` and `sysctl(8)` should then report that this is the
kernel in use:

```
# uname -i
GENERIC-MORELLO-PURECAP-BENCHMARK
# sysctl kern.bootfile
kern.bootfile: /boot/kernel.GENERIC-MORELLO-PURECAP-BENCHMARK/kernel
```

## Selecting another kernel from the boot loader

It is also possible to interactively select a different kernel from the boot
loader on the video or serial console:

```
    _____ _               _ ____   _____ _____
   / ____| |             (_)  _ \ / ____|  __ \
  | |    | |__   ___ _ __ _| |_) | (___ | |  | |
  | |    | '_ \ / _ \ '__| |  _ < \___ \| |  | |
  | |____| | | |  __/ |  | | |_) |____) | |__| |
   \_____|_| |_|\___|_|  |_|____/|_____/|_____/
                                                 ```                        `
                                                s` `.....---.......--.```   -/
 /---------- Welcome to CheriBSD ----------\    +o   .--`         /y:`      +.
 |                                         |     yo`:.            :o      `+-
 |  1. Boot Multi user [Enter]             |      y/               -/`   -o/
 |  2. Boot Single user                    |     .-                  ::/sy+:.
 |  3. Escape to loader prompt             |     /                     `--  /
 |  4. Reboot                              |    `:                          :`
 |  5. Cons: Serial                        |    `:                          :`
 |                                         |     /                          /
 |  Options:                               |     .-                        -.
 |  6. Kernel: kernel (1 of 6)             |      --                      -.
 |  7. Boot Options                        |       `:`                  `:`
 |                                         |         .--             `--.
 |                                         |            .---.....----.
 \-----------------------------------------/
```

Use the `6` key to rotate through kernel selections.
Once the desired kernel is selected, press the `1` key to proceed with the boot.
