# less

 The **less** is a program similar to `more`, but it has many more features. Less does not have to read the entire input file before starting, so with large input files it starts up faster than text editors like `vi`. Commands are based on both `more` and `vi`.

```sh
sudo less /var/log/syslog
```

Navigation    | |
--------------|--------------------------------------------------------------
`SPACE`   | forward one window
`b`       | backward one window
`d`       | forward half window
`u`       | backward half window
`j`       | navigate forward by one line
`10j`     | 10 lines forward
`k`       | navigate backward by one line
`10k`     | 10 lines backward
`G`       | go to the end of file
`g`       | go to the start of file
`q or ZZ` | exit the less pager

Search  | |
--------|---------------------------------------------------------------------
`/` | search for a pattern which will take you to the next occurrence
`?` | search for a pattern which will take you to the previous occurrence
`n` | for next match in forward
`N` | for previous match in backward

Other          | |
---------------|-----------------------------------------------------------------
`F`        | simulate tail `-f` inside less pager
`ma`       | mark the current position with the letter `a`
`'a`       | go to the marked position `a`
`&pattern` | display only the matching lines, not all
`v`        |using the configured editor edit the current file
`CTRL+G`   |  show the current file name along with line, byte and percentage statistics.
