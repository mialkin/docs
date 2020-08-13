# Linux

## OS

Command | Meaning
:-|:-
hostnamectl | OS info
cat /etc/*-release | OS name and version
uname -a | Kernel version

## Power managment

Command | Meaning
:-|:-
uptime | How long the system is running. <br/>This command returns set of values that involve, the current time, and the amount of time system is in running state, number of users currently logged into, and the load time for the past 1, 5 and 15 minutes respectively
sudo&nbsp;systemctl&nbsp;reboot | Reboot the system

## Network

Command | Meaning
:-|:-
curl ident.me | Public IP addresses of machine

## Storage

Command | Meaning
:-|:-
df -h . | Free disk space

## Shell

Command | Description
:-|:-
cat /etc/shells | Available shells
echo $SHELL | Display what shell the terminal opened with
bash | Switch to a bash or different shell type its name at the terminal
echo $0 | Currently used shell


## See also

[systemd](https://wiki.archlinux.org/index.php/systemd)
