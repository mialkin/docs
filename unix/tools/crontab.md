# crontab

The **cron** is a daemon to execute scheduled commands.

The **crontab**, short from cron table, is a file which contains the schedule of cron entries to be run and at specified times. File location varies by operating systems.

## Semantics

<pre>
# ┌───────────── minute (0 - 59)
# │ ┌───────────── hour (0 - 23)
# │ │ ┌───────────── day of the month (1 - 31)
# │ │ │ ┌───────────── month (1 - 12)
# │ │ │ │ ┌───────────── day of the week (0 - 6) (Sunday to Saturday; 7 is also Sunday on some systems)
# │ │ │ │ │
# │ │ │ │ │
# │ │ │ │ │
# * * * * * command to execute
</pre>

[↑ Cron expression generator](https://crontab.cronhub.io/).

| Expression    | Meaning                                     |
| ------------- | ------------------------------------------- |
| `* * * * *`   | Every minute                                |
| `*/5 * * * *` | Every 5 minutes                             |
| `0 * * * *`   | At minute 0                                 |
| `0 1 * * *`   | Every day at 01:00 AM                       |
| `0 0 */2 * *` | Every 2 days at 12:00 AM                    |
| `* 5 * * *`   | Every minute, between 05:00 AM and 05:59 AM |
| `0 */2 * * *` | Every 2 hours at the begging of the hour    |
| `* * 5 * *`   | Every minute, on day 5 of the month         |
| `* * * 5 *`   | Every minute, only in May                   |
| `* * * 5 1`   | Every minute, only on Monday, only in May   |
| `0 0 * * 1`   | Every Monday at 12:00 AM                    |
| `0 0 1 * *`   | Every first day of the month at 12:00 AM    |

[↑ Why does cron only offer minute granularity?](https://superuser.com/questions/620807/why-does-cron-only-offer-minute-granularity).

## Commands

| Command             | Description                 |
| ------------------- | --------------------------- |
| crontab -l          | Display the current crontab |
| crontab -e          | Edit the current crontab    |
| crontab -r          | Remove the current crontab  |
| crontab -u user2 -l | Show crontab for user2      |
| sudo crontab -l     | Show crontab for root       |

## Add cron job

```bash
cd /usr/local/bin
touch my_script.sh
chmod 744 my_script.sh 
vim my_script.sh
```

`my_script.sh` file:

```bash
#!/bin/bash

current_date_time="`date "+%Y-%m-%d %H:%M:%S"`";
echo $current_date_time >> /Users/user/Downloads/echo.txt;
```

```bash
crontab -e
```

Add this line to opened editor, save and close:

```text
* * * * * /usr/local/bin/my_script;
```

```bash
crontab -l
```

## Logs

To watch `cron`'s logs run:

```bash
tail -f /var/log/syslog
```
