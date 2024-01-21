# Kernel compilation modes

The CheriBSD kernel can be compiled either as hybrid or pure-capability code:

- The **hybrid kernel** enables capability use in userspace while making
  relatively little use of capabilities in the kernel's own implementation.
- The **pure-capability kernel** implements strong referential and spatial
  memory protection internally in the kernel, protecting against memory-safety
  vulnerabilities in components such as the network stack and system-call
  layer.

A number of precompiled kernels are provided in the CheriBSD release, enabling
or disabling debugging and revocation in combination with hybrid or
pure-capability compilations.
In general, we recommend that performance benchmarking be done only with
`NODEBUG` kernels; see [Benchmarking guidance](../benchmarking/README.md) for
further details.

## Precompiled kernels in CheriBSD 23.11

CheriBSD ships with a number of precompiled kernels that enable and disable
various relevant kernel configuration options.
Kernel configuration files themselves may be found in `sys/arm64/conf` in the
CheriBSD source tree, and will have names such as `GENERIC-MORELLO`.
The following kernel features may be selected from amongst the precompiled
kernels:

| Option name   | Description |
|---------------|-------------|
| `PURECAP`     | Compiled with internal memory safety |
| `NODEBUG`     | Compiled without kernel debugging features |
| `NOCAPREVOKE` | Compiled without userspace heap temporal safety |

The default kernel uses the `GENERIC-MORELLO` kernel
configuration, and is a hybrid kernel with debugging and userspace heap
temporal safety enabled.
It is unsuitable for benchmarking; instead, use a `NODEBUG` kernel
configuration.
The default kernel is expected to change to a pure-capability kernel in future
CheriBSD releases.

Kernel directory names are in the form `/boot/kernel*`; the default kernel is
named `/boot/kernel`.

```
# cd /boot
# ls -d kernel*
kernel
kernel.GENERIC-MORELLO-NOCAPREVOKE
kernel.GENERIC-MORELLO-NOCAPREVOKE-NODEBUG
kernel.GENERIC-MORELLO-NODEBUG
kernel.GENERIC-MORELLO-PURECAP
kernel.GENERIC-MORELLO-PURECAP-NOCAPREVOKE
kernel.GENERIC-MORELLO-PURECAP-NOCAPREVOKE-NODEBUG
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
GENERIC-MORELLO
```

## Selecting another kernel using loader.conf

You can set the kernel to boot via `/boot/loader.conf` using the variable
`kernel`.
This is set to the name of the kernel directory under `/boot`; for example,
the default is:

```
kernel="kernel"
```

You can persistently set the system to boot with a pure-capability kernel by
setting `kernel` as follows in `/boot/loader.conf`:

```
kernel="kernel.GENERIC-MORELLO-PURECAP"
```

After a reboot, `uname(1)` should then report that this is the kernel in use:

```
# uname -i
GENERIC-MORELLO-PURECAP
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
 |  6. Kernel: kernel (2 of 8)             |      --                      -.
 |  7. Boot Options                        |       `:`                  `:`
 |                                         |         .--             `--.
 |                                         |            .---.....----.
 \-----------------------------------------/
```

This can be done in one of two ways:

1. By rotating through kernel selections by using the `6` key.  Then press the
   `1' key to proceed with the boot.

2. By escaping to the loader prompt by hitting the `3` key, unloading the
   default kernel using the `unload kernel` command, and then booting the
   desired kernel using `boot /boot/kernel` (replacing the kernel name as
   required).
