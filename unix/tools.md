# Tools

## Table of contents

* [apt](#apt)
* [chmod](#chmod)

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
