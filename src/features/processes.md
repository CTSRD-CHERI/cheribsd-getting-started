# Userlevel process environments

CheriBSD 24.05 likewise supports three different userspace execution
environments:

- **Hybrid processes** provide strong binary compatibility with the non-CHERI
  version of the same architecture -- for example, aarch64 on Morello.

- **CheriABI processes** support pure-capability code execution.
  This process environment implements strong referential, spatial, and
  [heap temporal memory protection](temporal.md) through the system-call
  interface, dynamic linker, language runtime including heap memory
  allocators, and compiler-generated code.
  This protects against memory memory-safety vulnerabilities in both system
  services and applications.
  See [Building for CheriABI](../helloworld/index.html#building-for-cheriabi)
  for information on building for CheriABI.
  CheriABI is described in an [ASPLOS 2019
  paper](https://www.cl.cam.ac.uk/research/security/ctsrd/pdfs/201904-asplos-cheriabi.pdf).

- **Benchmark ABI processes** implement a code-generation model similar to
  CheriABI, but with relaxed program-counter capability bounds that enable
  improved performance prediction on the Arm Morello platform.
  See [Building for the Benchmark
  ABI](../helloworld/index.html#building-for-the-benchmark-abi).
  The Benchmark ABI is described in a [technical report on the topic from
  Arm and
  Cambridge](https://ctsrd-cheri.github.io/morello-early-performance-results/cover/index.html).

All of these environments can be used over either of the hybrid or
pure-capability kernels.
You can determine the process environment that a particular process is using
via the `procstat(1)` command, whose default output includes an `EMUL`
column indicating the process ABI:

```
% procstat -a
  PID  PPID  PGID   SID  TSID THR LOGIN    WCHAN     EMUL           COMM
...
 3197  3193  3193  3193     0   1 robert   select    FreeBSD ELF64C sshd
 3214  3170  3214  3170  3170   1 robert   select    FreeBSD ELF64  bash
...
```

In this example, the `sshd` daemon is using CheriABI (`FreeBSD ELF64C`), and
the `bash` command is using the Hybrid ABI (`FreeBSD ELF64`).

Pre-compiled third-party software applications (packages) are provided for
each of these ABIs, although CheriABI and Benchmark ABI packages are currently
considered experimental.  This is discussed further in the [chapter on
packages](../packages/).
