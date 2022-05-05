# Morello known issues

The following known issues may affect the reliability and usability of
CheriBSD on Morello:

- Due to a firmware bug, OS-triggered reboot may lead the system to hang.
  Use the "reboot" command on the management console to reboot the system.
- Due to a firmware bug, UEFI configuration variable values cannot be made
  persistent.  This affects, for example, the installation environment as the
  OS installer cannot instruct the firmware to boot only from the installed
  target, and not further USB sticks.
