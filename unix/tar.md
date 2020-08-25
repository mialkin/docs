# Tar

GNU **tar** is an archiving program designed to store multiple files in a single file (an **archive**), and to manipulate such archives.  The archive can be either a regular file or a device (e.g. a tape drive, hence the name of the program, which stands for **t**ape **ar**chiver), which can be located either on the local or on a remote machine.

## Commands

Command | Meaning
-|-
tar -cfv archive.tar.gz | Create uncompressed archive file `archive.tar`
tar -cfvz archive.tar.gz directory1 file1 | Create compressed archive file `archive.tar.gz` from `directory1` and `file1`
tar -fvxz archive.tar.gz | Extract `archive.tar.gz` archive file to current folder
tar -fvxz archive.tar.gz -C /path/to/dir | Extract `archive.tar.gz` archive to specific folder
tar -ftvz archive.tar.gz | List contents of `archive.tar.gz` archive file to stdout

### Options

Options to GNU tar can be given in three different styles. In **traditional style**, the first argument is a cluster of option letters and all subsequent arguments supply arguments to those options that require them. The arguments are read in the same order as the option letters. Any command line words that remain after all options has been processed are treated as non-optional arguments: file or archive member names.

For example, the `c` option requires creating the archive, the `v` option requests the verbose operation, and the `f` option takes an argument that sets the name of the archive to operate upon. The following command, written in the traditional style, instructs tar to store all files from the directory `/etc` into the archive file `etc.tar` verbosely listing the files being archived:

```bash
tar cfv etc.tar /etc
```

* `-c` — create new archive
* `-f` — use archive file (`archive.tar.gz`)
* `-t` — list archive contents to stdout
* `-v` — verbose output
* `-x` — extract to disk from the archive
* `-z` — filter the archive through gzip
* `-C` — changes the directory
* `--exclude <pattern>` — do not process files or directories that match the specified pattern

## See also

[tar(1) — Linux manual page](https://man7.org/linux/man-pages/man1/tar.1.html)
