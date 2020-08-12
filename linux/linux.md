# Linux

## OS information

Command | Meaning
-|-
`hostnamectl` | Check info about OS
`cat /etc/*-release` | Check OS name and version
`uname -a` | Check kernel version
|

## Power managment

Command | Meaning
-|-
uptime | Find out how long the system is running. <br/>This command returns set of values that involve, the current time, and the amount of time system is in running state, number of users currently logged into, and the load time for the past 1, 5 and 15 minutes respectively
sudo systemctl reboot | Shut down and reboot the system


## Disks

Check free disk space.

```sh
df -h .
```

## Network

Find public IP addresses of your machine.

```sh
curl ident.me
```



## See also

[systemd](https://wiki.archlinux.org/index.php/systemd)
