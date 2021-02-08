# Tools

- [Tools](#tools)
  - [apt](#apt)
  - [chmod](#chmod)
  - [chown](#chown)
  - [df](#df)
  - [du](#du)
  - [find](#find)
  - [htop](#htop)

## apt

The **apt** provides a high-level commandline interface for the package management system. It is intended as an end user interface and enables some options better suited for interactive usage by default compared to more specialized APT tools like `apt-get` and `apt-cache`.

| Command                  | Description                                                                 |
| ------------------------ | --------------------------------------------------------------------------- |
| apt install PACKAGE_NAME | Install specific package if it doesn't exist or upgrade it if it does exist |
| apt list --upgradeable   | List available updates but do not install them                              |
| apt search PACKAGE_NAME  | Search for a package                                                        |
| apt update               | Update list of available packages                                           |
| apt upgrade              | Update all installed packages                                               |

## chmod

**chmod** is the command and system call which is used to change the access permissions of file system objects (files and directories). The name is an abbreviation of _change mode_.

| Command               | Description                     |
| --------------------- | ------------------------------- |
| chmod 777 FOLDER_NAME | Change folder access permission |

[↑ chmod on Wikipedia](https://en.wikipedia.org/wiki/Chmod)

## chown

| Command                        | Description                    |
| ------------------------------ | ------------------------------ |
| chown -R USER_NAME FOLDER_NAME | Change the owner of the folder |

## df

Display free disk space.

| Command | Description                                                          |
| ------- | -------------------------------------------------------------------- |
| df -h . | Dispaly free disk space in human-readable form for current directory |

## du

Display disk usage statistics.

| Command | Description                                                                |
| ------- | -------------------------------------------------------------------------- |
| du -h . | Dispaly disk usage statistics in human-readable form for current directory |

## find

Find is a command-line utility that locates files based on some user-specified criteria and then applies some requested action on each matched object.

| Command                | Description                                                                 |
| ---------------------- | --------------------------------------------------------------------------- |
| find . -name YOUR_NAME | Find inside current directory all files/folders that match `YOUR_NAME` name |

## htop

| Command          | Description                     |
| ---------------- | ------------------------------- |
| <kbd>\\</kbd>    | filter                          |
| <kbd>/</kbd>     | search                          |
| <kbd>Space</kbd> | select/deselect a single record |
| <kbd>u</kbd>     | filter processes by user        |
| <kbd>k</kbd>     | send command to process         |
| <kbd>U</kbd>     | deselect all records            |
| <kbd>M</kbd>     | sort by memory                  |
| <kbd>P</kbd>     | sort by CPU                     |
