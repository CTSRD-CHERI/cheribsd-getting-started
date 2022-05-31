# CheriBSD on an Arm Morello board

Arm's Morello board contains a quadcore CHERI-extended Neoverse N1 processor
design, as well as two management microcontrollers.
CheriBSD is able to run on, and fully utilize, Morello's CHERI support to
implement fine-grained kernel and userspace memory safety, and on experimental
branches, also implement scalable software compartmentalization.
You can install CheriBSD into the hard disk in your Morello box using a USB
stick installer image.
You will first need to upgrade the firmware, as older firmware revisions
contain bugs or lack features essential to running the OS.
This chapter explains how to:

- Connect to the Morello management and system consoles via USB,
- Upgrade the board's firmware, and
- Install CheriBSD from a USB stick.

It is also possible to run CheriBSD from a USB stick, or netboot CheriBSD on
the board, but those topics are not currently covered by this guide.

**It is important that the most recent Arm-provided firmware be installed on
your Morello board, as the board has most likely shipped with an older
firmware version.
If you are not running the latest version, you may experience hard-to-diagnose
behaviour or reliability problems.**
