# Kernel compilation modes

The CheriBSD kernel can be compiled either as hybrid or pure-capability code:

- The **hybrid kernel** enables capability use in userspace while making
  relatively little use of capabilities in the kernel's own implementation.
- The **pure-capability kernel** implements strong referential and spatial
  memory protection internally in the kernel, protecting against memory-safety
  vulnerabilities in components such as the network stack and system-call
  layer.
