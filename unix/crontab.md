# crontab

The **cron** is a daemon to execute scheduled commands.

The **crontab** (CRON TABle) is a file which contains the schedule of cron entries to be run and at specified times. File location varies by operating systems

Command | Description
-|-
crontab -l | Display the current crontab
crontab -r | Remove the current crontab
crontab -e | Edit the current crontab
crontab -u user2 -l | Show crontab for user2
sudo crontab -l | Show crontab for root

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

## See also

* [In the shell, what does “ 2>&1 ” mean?](https://stackoverflow.com/questions/818255/in-the-shell-what-does-21-mean)
