# Morello known issues

The following known issues may affect the reliability and usability of
CheriBSD on Morello:

- Due to a firmware bug, OS-triggered reboot may lead the system to hang.
  Use the "reboot" command on the management console to reboot the system.
- Due to a firmware bug, some earlier firmware versions are not able to
  persist UEFI configuration variables.
  This affects, for example, the installation environment as the OS installer
  cannot instruct the firmware to boot only from the installed target, and not
  further USB sticks.

**It is extremely important that you use the most recent firwmare with your
Morello board, to avoid these and other issues present in earlier firmware
releases.**
