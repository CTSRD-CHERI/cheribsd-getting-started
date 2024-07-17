# Userlevel heap temporal memory safety (experimental)

CheriBSD 24.05 incorporates support for userlevel heap temporal memory
safety based on a load-barrier extension to
[Cornucopia](https://www.cl.cam.ac.uk/research/security/ctsrd/pdfs/2020oakland-cornucopia.pdf),
which is inspired by garbage-collection techniques.
A forthcoming paper at ASPLOS 2024 on the Cornucopia Reloaded technique will
describe these differences in detail.

This feature involves a collaboration between the kernel (which provides
asynchronous capability revocation with VM acceleration) and the userlevel
heap allocator (which quarantines freed memory until revocation of any
pointers to it) to ensure that memory cannot be reallocated until there are
no outstanding valid capabilities lasting from its previous allocation.
The userlevel memory allocator and the kernel revoker share an `epoch counter`
that counts the number of completed atomic revocation sweeps.
Memory added to quarantine in one epoch cannot be removed from quarantine
until at least one complete epoch has passed -- i.e., the epoch counter has
been increased by 2.
More information on temporal memory safety support can be found in the
[mrs(3) man page](https://man.cheribsd.org/cgi-bin/man.cgi/dev/mrs):

```
man mrs
```

## Checking whether temporal safety is globally enabled

Use the `sysctl(8)` command to inspect the value of the
`security.cheri.runtime_revocation_default` system MIB entry:

```
sysctl security.cheri.runtime_revocation_default
```

This sysctl sets the default policy for revocation used by processes on
startup.
We recommend setting this in `/boot/loader.conf`, which is processed by the
boot loader before any user processes start.

## Controlling revocation by binary or process

You can forcefully enable or disable revocations for a specific binary or
process
with
[elfctl(1)](https://man.cheribsd.org/cgi-bin/man.cgi/dev/elfctl)
or
[proccontrol(1)](https://man.cheribsd.org/cgi-bin/man.cgi/dev/proccontrol)
and ignore the default policy:

```
elfctl -e <+cherirevoke or +nocherirevoke> <binary>
```

```
proccontrol -m cherirevoke -s <enable or disable> <program with arguments>
```

You can read more on these commands in the
[mrs(3) man page](https://man.cheribsd.org/cgi-bin/man.cgi/dev/mrs).

## Synchronous revocation

Revocation normally occurs asynchronously, with a memory quarantine preventing
memory reuse until revocation of any pointers to that memory.
You can insert a specific call in your program to await synchronous
revocation:

```
malloc_revoke();
```

Synchronous revocation on every call to `free()` can be configured using the
`_RUNTIME_REVOCATION_EVERY_FREE_ENABLE` environmental variable.

Note that synchronous revocation can incur extremely high expense.

## Monitoring revocation in processes

Use the `procstat cheri -v` command to inspect the CHERI memory-safety state
of a target process.
For example:

```
# procstat cheri -v 923 1012
  PID COMM                C QUAR  RSTATE                              EPOCH
  923 seatd               P  yes    none                                  0
 1012 Xorg                P  yes    none                               0xd2
```

Both processes in this example use a pure-capability process environment, have
quarantining enabled, and are not currently revoking.
seatd has never needed to perform a revocation pass, as it remains in epoch 0,
whereas X.org has a non-zero epoch and has performed multiple passes.
