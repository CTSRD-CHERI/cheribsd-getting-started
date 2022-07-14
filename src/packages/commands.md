# Useful commands

The following list includes useful commands to manage packages.
Replace `pkg` with `pkg64c` to execute a command in the context of CheriABI
packages and with `pkg64` in the context of hybrid ABI packages.

* List available package manager commands:
  ```
  pkg help
  ```
* Read more information on a package manager command:
  ```
  pkg help <command>
  ```
* Search a package repository:
  ```
  pkg search <pattern>
  ```
* Search a package repository and display full information:
  ```
  pkg search --full <pattern>
  ```
* List installed packages:
  ```
  pkg info
  ```
* Display information for an installed package:
  ```
  pkg info <pkg-name>
  ```
* Install a package:
  ```
  pkg install <pkg-name>
  ```
* Delete a package:
  ```
  pkg delete <pkg-name>
  ```
* Delete packages that are no longer required:
  ```
  pkg autoremove
  ```
* Check a new package version for an installed package:
  ```
  pkg upgrade -n <pkg-name>
  ```
* Check new package versions for all installed packages:
  ```
  pkg upgrade -n
  ```
* Upgrade a package after confirmation:
  ```
  pkg upgrade <pkg-name>
  ```
* Upgrade all packages after confirmation:
  ```
  pkg upgrade
  ```
* Display which package installed a file:
  ```
  pkg which /path/to/file
  ```
* Check a number of installed packages:
  ```
  pkg stats -l
  ```
* Check a number of available packages:
  ```
  pkg stats -r
  ```
