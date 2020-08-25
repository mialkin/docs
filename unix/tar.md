# Tar

The **tar** is a command that manipulates tape archives.

## Commands

Command | Meaning
-|-
tar -cfv archive.tar.gz | Create uncompressed archive file `archive.tar`
tar -cfvz archive.tar.gz directory1 file1 | Create compressed archive file `archive.tar.gz` from `directory1` and `file1`
tar -fvxz archive.tar.gz | Extract archive to current folder
tar -fvxz archive.tar.gz -C /path/to/dir | Extract archive to specific folder
tar -ftvz | List contents of `archive.tar.gz` archive file to stdout

### Options

* `-c` — create new archive
* `-f` — use archive file (`archive.tar.gz`)
* `-t` — list archive contents to stdout
* `-v` — verbose output
* `-x` — extract to disk from the archive
* `-z` — filter the archive through gzip
* `-C` — changes the directory
* `--exclude <pattern>` — do not process files or directories that match the specified pattern
 