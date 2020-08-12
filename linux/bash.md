# Linux

## OS

Command | Meaning
-|-
hostnamectl | Info about OS
cat /etc/*-release | OS name and version
uname -a | Kernel version

## Power managment

Command | Meaning
-|-
uptime | How long the system is running. <br/>This command returns set of values that involve, the current time, and the amount of time system is in running state, number of users currently logged into, and the load time for the past 1, 5 and 15 minutes respectively
sudo&nbsp;systemctl&nbsp;reboot | Shut down and reboot the system

## Network

Command | Meaning
-|-
curl ident.me | Public IP addresses of your machine

## Storage

Command | Meaning
-|-
df -h . | Free disk space

## See also

[systemd](https://wiki.archlinux.org/index.php/systemd)
