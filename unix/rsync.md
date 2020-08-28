# rsync

The **rsync** is a fast and extraordinarily versatile file copying tool. It can copy locally, to/from another host over any remote shell, or to/from a remote rsync daemon. It is famous for its delta-transfer algorithm, which reduces the amount of data sent over the network by sending only the differences between the source files and the existing files in the destination.

Rsync is widely used for backups and mirroring and as an improved copy command for everyday use.

Rsync finds files that need to be transferred using a "quick check" algorithm (by default) that looks for files that have changed in size or in last-modified time.

## Commands

Command | Description
-|-
rsync Original/* Backup/ | Copies all files inside Original directory to Backup directory
rsync -r Original/ Backup/ | Copies recursively all files and folders inside Original directory to Backup directory
rsync -r Original Backup/ | Copies entire Original directory recursively, not just its content
rsync -av Original/ Backup/ | The same as previous. `v` stands for verbose. `a` stands for "archive"; it copies everything recursively preserving information about file permissions and other things
rsync -av --dry-run Original/ Backup/|  Displays what files would have copied without actually doing anything
rsync -zaP Original user@host.com:/where/to | Transfers entire Original directory to remote (write Original/ to transfer only folder's content instead) `z` stands for compress, `P` means show progress

## See also

* [How To Use The rsync Command - Sync Files Locally and Remotely](https://www.youtube.com/watch?v=qE77MbDnljA)