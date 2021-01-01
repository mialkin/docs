# apt

The **apt** provides a high-level commandline interface for the package management system. It is intended as an end user interface and enables some options better suited for interactive usage by default compared to more specialized APT tools like `apt-get` and `apt-cache`.

## Shortcuts

| Command                 | Description                                                                 |
| ----------------------- | --------------------------------------------------------------------------- |
| apt update              | Update list of available packages                                           |
| apt list --upgradeable  | List available updates but do not install them                              |
| apt upgrade             | Update all installed packages                                               |
| apt install \<package\> | Install specific package if it doesn't exist or upgrade it if it does exist |
| apt search \<package\>  | Search for a package. You can also use `apt-cache search <package>` command |
