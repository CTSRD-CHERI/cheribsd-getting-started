# Updating an existing system

The
[cheribsd-update](https://man.cheribsd.org/cgi-bin/man.cgi/release-26.07/cheribsd-update.8)
tool, available as an optional installable package, can be used to update an
existing CheriBSD 25.03 installation to 26.07.
Due to CheriBSD's evolving ABI, backwards compatibility is only ensured for
consecutive releases, and so intermediate releases cannot be skipped when
upgrading an existing CheriBSD installation.

***Although every effort has been made to handle errors and corner cases, bugs
may exist, and so to avoid possible data loss you should back up any important
system contents before following these steps.***

In order to use cheribsd-update, you must first install it using:

```
pkg64c install cheribsd-update
```

Once installed, you can start the update process using:

```
cheribsd-update update 25.03 26.07
```

This step will download quite a few large files to perform the update, and so
may take some time.
Once downloaded, it will inspect the running system, and outline the system
components that will be updated, added, and removed.
For an update from 25.03 to 26.07 this should look as follows:

```
--------------------------------------------------------------
>>> Confirm update
--------------------------------------------------------------
The following distribution sets will be updated:
    base
    base-dbg
    kernel
    kernel-dbg
    kernel.GENERIC-MORELLO
    kernel.GENERIC-MORELLO-NODEBUG
    kernel.GENERIC-MORELLO-NODEBUG-dbg
    kernel.GENERIC-MORELLO-PURECAP-BENCHMARK
    kernel.GENERIC-MORELLO-PURECAP-BENCHMARK-NODEBUG
    kernel.GENERIC-MORELLO-PURECAP-BENCHMARK-NODEBUG-dbg
    kernel.GENERIC-MORELLO-PURECAP-BENCHMARK-dbg
    kernel.GENERIC-MORELLO-PURECAP-NODEBUG
    kernel.GENERIC-MORELLO-PURECAP-NODEBUG-dbg
    kernel.GENERIC-MORELLO-dbg
    lib32
    lib32-dbg
    lib64
    lib64-dbg
    lib64cb
    lib64cb-dbg
    tests

The following distribution sets will be removed:
    kernel.GENERIC-MORELLO-NOCAPREVOKE
    kernel.GENERIC-MORELLO-NOCAPREVOKE-NODEBUG
    kernel.GENERIC-MORELLO-NOCAPREVOKE-NODEBUG-dbg
    kernel.GENERIC-MORELLO-NOCAPREVOKE-dbg
    kernel.GENERIC-MORELLO-PURECAP-BENCHMARK-NOCAPREVOKE
    kernel.GENERIC-MORELLO-PURECAP-BENCHMARK-NOCAPREVOKE-NODEBUG
    kernel.GENERIC-MORELLO-PURECAP-BENCHMARK-NOCAPREVOKE-NODEBUG-dbg
    kernel.GENERIC-MORELLO-PURECAP-BENCHMARK-NOCAPREVOKE-dbg
    kernel.GENERIC-MORELLO-PURECAP-NOCAPREVOKE
    kernel.GENERIC-MORELLO-PURECAP-NOCAPREVOKE-NODEBUG
    kernel.GENERIC-MORELLO-PURECAP-NOCAPREVOKE-NODEBUG-dbg
    kernel.GENERIC-MORELLO-PURECAP-NOCAPREVOKE-dbg

Proceed? [Y/n] 
```

If this looks as expected, confirm at the prompt with the Enter key.
This will update the kernel, at which point you will be asked to reboot and
resume the update:

```
--------------------------------------------------------------
>>> Reboot required; please reboot then run:
>>>     cheribsd-update [-D destdir] resume
>>> to continue updating
--------------------------------------------------------------
```

Once rebooted, log in as usual.
As part of updating the rest of the system, this next step will remove old
versions of libraries, some of which may be required by tools such as sudo.
If you are not able to log in as the root user directly but instead use sudo,
it is recommended that you use a root shell via <code>sudo -i</code>, as you
may not be able to use sudo immediately after the update.

With a suitable shell, resume and finish the update with:

```
cheribsd-update resume
```

If there are conflicts when updating configuration files you will be asked to
resolve them in turn:

```
--------------------------------------------------------------
>>> Running etcupdate resolve
--------------------------------------------------------------
Resolving conflict in '/etc/devd.conf':
Select: (p) postpone, (df) diff-full, (e) edit,
        (h) help for more options: 
```

Once finished, cheribsd-update should give the following message:

```
--------------------------------------------------------------
>>> Update finished; please upgrade any installed packages
--------------------------------------------------------------
```

You should now update your packages with:

```
pkg64c bootstrap -f
pkg64c upgrade
```
(NB: bootstrap -f reinstalls the package manager, in case it requires removed
library versions)

Repeat this bootstrap and upgrade process for any other package ABIs installed.
Restart once more to ensure all system processes are running the new version.
