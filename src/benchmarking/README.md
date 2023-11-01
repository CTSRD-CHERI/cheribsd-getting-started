# Benchmarking guidance

CheriBSD is a research operating system designed to run on experimental
hardware.
In this section, we provide high-level guidance on configuring CheriBSD for
software benchmarking.

# Framing

The expectation of this section is that a performance comparison is being
made between baseline aarch64-compiled software (64-bit Arm) and memory-safe
aarch64c software (CheriABI) on the Arm Morello board.

Before proceeding to the remainder of this section, it is essential to first
read [Early performance results from the prototype Morello
microarchitecture](https://ctsrd-cheri.github.io/morello-early-performance-results/cover/index.html).
That document provides detailed information on how to interpret performance
measurements, including documenting known limitations of the prototype
Morello microarchitecture.

A key conclusion from that work is that CheriABI software being used for
performance measurement should be compiled for the aarch64cb "Benchmark ABI",
and not aarch64c.

# Kernel configuration

Depending on the version of CheriBSD you are using, the kernel may have a
number of debugging fetures enabled.
These features can substantially impact system performance, including
inducing disproportionate performance overhead for specific system behaviours.
For example, enabling kernel lock-order checking ("WITNESS") will introduce
substantial overhead, and in particular will impact kernel-centric workloads
that make more intensive use of locks, such as networking.
During boot, CheriBSD will add the following lines to `/etc/motd`, which will
be displayed at login, to warn you about the "INVARIANTS" and "WITNESS"
debugging features:

```
WARNING: INVARIANTS kernel option defined, expect reduced performance
WARNING: WITNESS kernel option defined, expect reduced performance
```

Performance measurements employing a hybrid kernel should use the
`kernel.GENERIC-MORELLO-NODEBUG` configuration when kernel memory safety is
not required.
The following line can be added to `/boot/loader.conf`:

```
kernel="kernel.GENERIC-MORELLO-NODEBUG"
```

Measurements requiring a memory-safe kernel should use the
`kernel.GENERIC-MORELLO-PURECAP-NODEBUG` configuration.
The following can be added to `/boot/loader.conf`:

```
kernel="kernel.GENERIC-MORELLO-PURECAP-NODEBUG"
```

You will need to reboot in order for this change to take effect.

# Heap temporal memory safety

As of CheriBSD 23.11, userlevel heap temporal safety is enabled by default.
This support is experimental, and has not yet been through significant
performance analysis and optimization.
We recommend disabling temporal safety support for the full system during
benchmarking not specifically intended to capture temporal safety
performance.
During boot, CheriBSD will add the following line to `/etc/motd`, which will
be displayed at login, to warn you about the temporal safety feature:

```
WARNING: capability revocation enabled by default, this may affect performance
```

The following line can be added to `/boot/loader.conf`:

```
security.cheri.runtime_quarantine_default=0
```

You will need to reboot in order for this change to take effect.

# The Benchmark ABI

As of CheriBSD 23.11, CheriBSD supports the Benchmark ABI, a modified form of
code generation improving performance on the Arm Morello board.
This is required due to limitations on bounds prediction in the current
Morello prototype, which would be resolved in a production microarchitecture.
The performance of the Benchmark ABI is more predictive of potential future
CHERI microarchitectural performance than the Morello prototype running
software compiled for CheriABI, making it preferable for benchmarking.
However, this comes at the cost of reduce security, and so software compiled
for the Benchmark ABI should not be used for security evaluation.
Programs may be compiled to the Benchmark ABI using the
`-mabi=purecap-benchmark` command-line argument to `cc`.

More information on compiling with the Benchmark ABI can be found in
[Compiling Hello World](../helloworld/).
More information on what the Benchmark ABI is, and how to interpret
performance results, can be found in [Early performance results from the
prototype Morello
microarchitecture](https://ctsrd-cheri.github.io/morello-early-performance-results/cover/index.html).
