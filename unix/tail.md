# tail

The **tail** command outputs the last part of files.

By default tail prints  the  last  10  lines of each specified file to standard output.

Command | Description
-|-
tail -f /var/log/syslog | `-f` — output appended data as the file grows
tail -n 20 /var/log/syslog | `-n` — output the last n lines

## See also

* [Is `tail -f` more efficient than `less +F`](https://unix.stackexchange.com/questions/193620/is-tail-f-more-efficient-than-less-f)?
