# Morello known issues

The following known issues may affect the reliability and usability of
CheriBSD on Morello:

- Due to a firmware bug, OS-triggered reboot may lead the system to hang when
  using older firmware.
  Update your firmware or use the "reboot" command on the management console to
  reboot the system once the OS has shut down.
- Due to a firmware bug, some earlier firmware versions are not able to
  persist UEFI configuration variables.
  This affects, for example, the installation environment as the OS installer
  cannot instruct the firmware to boot only from the installed target, and not
  further USB sticks.
- Due to a firmware bug, the system sometimes corrupts the AP QSPI flash.
  This appears to the user as hang on the morello console with an assertion
  like:
  ```
  Loading driver at 0x000F9490000 EntryPoint=0x000F94A003C VariableRuntimeDxe.efi
  ASSERT [VariableRuntimeDxe] VariableParsing.c(502): NameSizeOfVariable (PtrTrack->CurrPtr, AuthFormat) != 0
  ```
  To recover from this error, run following commands at the mcc console:
  `debug`, `selftest`, `R`, `0`, `exit`, `reboot`.

**It is extremely important that you use the most recent firmware with your
Morello board, to avoid these and other issues present in earlier firmware
releases.**
