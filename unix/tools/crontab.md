# crontab

The **cron** is a daemon to execute scheduled commands.

The **crontab**, short from cron table, is a file which contains the schedule of cron entries to be run and at specified times. File location varies by operating systems.

## Semantics

<pre>
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday;
# │ │ │ │ │                                   7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# * * * * * command to execute
</pre>

## Commands

| Command             | Description                 |
| ------------------- | --------------------------- |
| crontab -l          | Display the current crontab |
| crontab -e          | Edit the current crontab    |
| crontab -r          | Remove the current crontab  |
| crontab -u user2 -l | Show crontab for user2      |
| sudo crontab -l     | Show crontab for root       |

## Logs

To watch `cron`'s logs run:

```bash
tail -f /var/log/syslog
```
