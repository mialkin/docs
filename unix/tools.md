# Tools

- [Tools](#tools)
  - [apt](#apt)
  - [chmod](#chmod)
  - [chown](#chown)
  - [curl](#curl)
    - [Download a file](#download-a-file)
    - [POST (HTTP)](#post-http)
  - [dd](#dd)
  - [df](#df)
  - [du](#du)
  - [find](#find)
  - [htop](#htop)
  - [iproute2](#iproute2)
  - [less](#less)
  - [rg](#rg)
  - [rsync](#rsync)
  - [scp](#scp)
  - [systemctl](#systemctl)
  - [tail](#tail)
  - [tar](#tar)
  - [xxd](#xxd)

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

**Modes** are the filesystem permissions given to _user_ (i.e. the file owner), _group_ (i.e. the group owning the file) and _others_ (users who are not the owner of the file and who are not members of the group) classes to access files under Unix.

References:

- u — user, file owner
- g — group, members of the file's group
- o — others, users who are neither the file's owner nor memebers of the file's group
- a — all, all three of the above, same as `ugo`

Operators:

- \+ — adds the specified modes to the specified classes
- \- — removes the specified modes from the specified classes
- = — the modes specified are to be made the exact modes for the specified classes

Modes:

- r — read, read a file or list a directory's contents
- w — write, write to a file or directory
- x — execute, execute a file or recurse a directory tree

| Command               | Description                        |
| --------------------- | ---------------------------------- |
| chmod +x FILE_NAME    | Add execute mode to all classes    |
| chmod a-rwx FILE_NAME | Removes all modes from all classes |
| chmod 100 FILE_NAME   | Adds _only_ execute mode to user   |
| chmod 200 FILE_NAME   | Adds _only_ write mode to user     |
| chmod 400 FILE_NAME   | Adds _only_ read mode to user      |

[↑ chmod on Wikipedia](https://en.wikipedia.org/wiki/Chmod)

## chown

| Command                        | Description                    |
| ------------------------------ | ------------------------------ |
| chown -R USER_NAME FOLDER_NAME | Change the owner of the folder |

## curl

| Command                                   | Description                         |
| ----------------------------------------- | ----------------------------------- |
| curl `https://www.example.com`            | Get the main page from a web-server |
| curl -H "Host: sub.domain.com" 120.7.3.15 | Passing header value                |
| curl -v `https://www.example.com`         | Get verbose fetching                |

### Download a file

Get a web page and store in a local file with a specific name:

```bash
curl -o thatpage.html `http://www.example.com`
```

Get a web page and store in a local file, make the local file get the name of the remote document (if no file name part is specified in the URL, this will fail):

```bash
curl -O http://www.example.com/index.html
```

### POST (HTTP)

Post a simple "name" and "phone" guestbook (the post data must be urlencoded):

```bash
curl -d "name=Rafael%20Sagula&phone=3320780" http://www.where.com/guest.cgi
```

## dd

Copies the standard input to the standard output.

Input data is read and written in 512-byte blocks. If input reads are short, input from multiple reads are aggregated to form the output block. When finished, `dd` displays the number of complete and partial input and output blocks and truncated input records to the standard error output.

Generate a file exactly 24MB in size:

```bash
dd if=/dev/zero of=output.dat bs=1m count=24
```

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

| Command                       | Description                                                                 |
| ----------------------------- | --------------------------------------------------------------------------- |
| find . -name YOUR_NAME        | Find inside current directory all files/folders that match `YOUR_NAME` name |
| find . -type f -name "\*.txt" | Find inside current directory all files with the extension `.txt`           |

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

## iproute2

**iproute2** is a collection of utilities for controlling and monitoring various aspects of networking in the Linux kernel, including routing, network interfaces, tunnels, traffic control, and network-related device drivers.

iproute2 collection contains the following command-line utilities: **ip**, **ss**, **bridge**, **rtacct**, **rtmon**, **tc**, **ctstat**, **lnstat**, **nstat**, **routef**, **routel**, **rtstat**, **tipc**, **arpd** and **devlink**.

## less

The **less** is a program similar to `more`, but it has many more features. Less does not have to read the entire input file before starting, so with large input files it starts up faster than text editors like `vi`. Commands are based on both `more` and `vi`.

```sh
sudo less /var/log/syslog
```

| Navigation |                               |
| ---------- | ----------------------------- |
| `SPACE`    | forward one window            |
| `b`        | backward one window           |
| `d`        | forward half window           |
| `u`        | backward half window          |
| `j`        | navigate forward by one line  |
| `10j`      | 10 lines forward              |
| `k`        | navigate backward by one line |
| `10k`      | 10 lines backward             |
| `G`        | go to the end of file         |
| `g`        | go to the start of file       |
| `q or ZZ`  | exit the less pager           |

| Search |                                                                     |
| ------ | ------------------------------------------------------------------- |
| `/`    | search for a pattern which will take you to the next occurrence     |
| `?`    | search for a pattern which will take you to the previous occurrence |
| `n`    | for next match in forward                                           |
| `N`    | for previous match in backward                                      |

| Other      |                                                                             |
| ---------- | --------------------------------------------------------------------------- |
| `F`        | simulate tail `-f` inside less pager                                        |
| `ma`       | mark the current position with the letter `a`                               |
| `'a`       | go to the marked position `a`                                               |
| `&pattern` | display only the matching lines, not all                                    |
| `v`        | using the configured editor edit the current file                           |
| `CTRL+G`   | show the current file name along with line, byte and percentage statistics. |

## rg

The **rg** tool allows to search current directory recursively for lines matching a pattern.

Search "ssh" keyword.

```sh
rg text_in_file
rg -l filename_text
```

## rsync

The **rsync** is a fast and extraordinarily versatile file copying tool. It can copy locally, to/from another host over any remote shell, or to/from a remote rsync daemon. It is famous for its delta-transfer algorithm, which reduces the amount of data sent over the network by sending only the differences between the source files and the existing files in the destination.

Rsync is widely used for backups and mirroring and as an improved copy command for everyday use.

Rsync finds files that need to be transferred using a "quick check" algorithm (by default) that looks for files that have changed in size or in last-modified time.

| Command                                     | Description                                                                                                                                                             |
| ------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| rsync Original/\* Backup/                   | Copies all files inside Original directory to Backup directory                                                                                                          |
| rsync -r Original/ Backup/                  | Copies recursively all files and folders inside Original directory to Backup directory                                                                                  |
| rsync -r Original Backup/                   | Copies entire Original directory recursively, not just its content                                                                                                      |
| rsync -av Original/ Backup/                 | The same as previous. `v` stands for verbose. `a` stands for "archive"; it copies everything recursively preserving information about file permissions and other things |
| rsync -av --dry-run Original/ Backup/       | Displays what files would have copied without actually doing anything                                                                                                   |
| rsync -zaP Original user@host.com:/where/to | Transfers entire Original directory to remote (write Original/ to transfer only folder's content instead) `z` stands for compress, `P` means show progress              |

Example:

```bash
rsync -v user@domain.ru:/var/file.db /Users/aleksei/Backups/file-`date +'%Y-%m-%d'`.db
```

## scp

Secure copy (remote file copy program); scp copies files between hosts on a network. It uses ssh for data transfer, and uses the same authentication and provides the same security as ssh. scp will ask for passwords or passphrases if they are needed for authentication.

local → remote:

```sh
scp /file/to/send username@remote:/where/to/put
```

remote → local:

Copy from remote to local:

```sh
scp username@remote:/file/to/send /where/to/put
```

## systemctl

The **systemctl** is a controlling interface and inspection tool for service manager systemd.

The **systemd** is the mother of all processes, responsible for bringing the Linux host up to a state where productive work can be done.

| Command               | Meaning           |
| --------------------- | ----------------- |
| sudo systemctl reboot | Reboot the system |

## tail

The **tail** command outputs the last part of files.

By default tail prints the last 10 lines of each specified file to standard output.

| Command                    | Description                                   |
| -------------------------- | --------------------------------------------- |
| tail -f /var/log/syslog    | `-f` — output appended data as the file grows |
| tail -n 20 /var/log/syslog | `-n` — output the last n lines                |

## tar

GNU **tar** is an archiving program designed to store multiple files in a single file (an **archive**), and to manipulate such archives. The archive can be either a regular file or a device (e.g. a tape drive, hence the name of the program, which stands for **t**ape **ar**chiver), which can be located either on the local or on a remote machine.

| Command                                 | Explanation                                                                   |
| --------------------------------------- | ----------------------------------------------------------------------------- |
| tar cf archive.tar                      | Create uncompressed archive file `archive.tar`                                |
| tar czf archive.tar.gz directory1 file1 | Create compressed archive file `archive.tar.gz` from `directory1` and `file1` |
| tar xf archive.tar.gz                   | Extract `archive.tar.gz` archive file to current folder                       |
| tar xf archive.tar.gz -C /path/to/dir   | Extract `archive.tar.gz` archive to specific folder                           |
| tar tf archive.tar                      | List contents of `archive.tar` archive file to stdout                         |

**Options**:

- `c` — create new archive
- `f` — use archive file (`archive.tar.gz`)
- `t` — list archive contents to stdout
- `v` — verbose output
- `x` — extract to disk from the archive
- `z` — filter the archive through gzip
- `-C` — changes the directory
- `--exclude <pattern>` — do not process files or directories that match the specified pattern

## xxd

Creates a hex dump of a given file or standard input. It can also convert a hex dump back to its original binary form.

```bash
xxd filename | less
```
