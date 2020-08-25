# tar

GNU **tar** is an archiving program designed to store multiple files in a single file (an **archive**), and to manipulate such archives.  The archive can be either a regular file or a device (e.g. a tape drive, hence the name of the program, which stands for **t**ape **ar**chiver), which can be located either on the local or on a remote machine.

## Commands

Command | Meaning
-|-
tar cf archive.tar | Create uncompressed archive file `archive.tar`
tar czf archive.tar.gz directory1 file1 | Create compressed archive file `archive.tar.gz` from `directory1` and `file1`
tar xf archive.tar.gz | Extract `archive.tar.gz` archive file to current folder
tar xf archive.tar.gz -C /path/to/dir | Extract `archive.tar.gz` archive to specific folder
tar tf archive.tar | List contents of `archive.tar` archive file to stdout

### Options

* `c` — create new archive
* `f` — use archive file (`archive.tar.gz`)
* `t` — list archive contents to stdout
* `v` — verbose output
* `x` — extract to disk from the archive
* `z` — filter the archive through gzip
* `-C` — changes the directory
* `--exclude <pattern>` — do not process files or directories that match the specified pattern

## See also

[tar(1) — Linux manual page](https://man7.org/linux/man-pages/man1/tar.1.html)
