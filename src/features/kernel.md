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
  security.  The benchmark kernel is considered appropriate for a performance
  evaluation of the Arm Morello architecture and third-party software
  executed on it. It should not be used for a security evaluation and the
  pure-capability kernel should be used instead.
