# Commands

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

## Power managment

### Uptime

Find out how long the system is running. This command returns set of values that involve, the current time, and the amount of time system is in running state, number of users currently logged into, and the load time for the past 1, 5 and 15 minutes respectively.

```sh
uptime
```

### Reboot

Shut down and reboot the system.

```sh
sudo systemctl reboot
```

## See also

[systemd](https://wiki.archlinux.org/index.php/systemd)
