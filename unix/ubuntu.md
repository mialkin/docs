# Ubuntu

## Table of contents

- [Ubuntu](#ubuntu)
  - [Table of contents](#table-of-contents)
  - [VPS harderning](#vps-harderning)
  - [Create a user and allow him to connect via SSH](#create-a-user-and-allow-him-to-connect-via-ssh)
  - [Change SSH port and disable password authentication](#change-ssh-port-and-disable-password-authentication)
  - [Watch temperature](#watch-temperature)
    - [CPU](#cpu)
    - [SSD](#ssd)
  - [Enable/disable GUI for Ubuntu Desktop](#enabledisable-gui-for-ubuntu-desktop)
  - [Enable/disable Wi-Fi](#enabledisable-wi-fi)
  - [User management](#user-management)
    - [Last logged in users](#last-logged-in-users)
    - [List users](#list-users)
    - [Elevate user's permissions](#elevate-users-permissions)
    - [List all user groups](#list-all-user-groups)
  - [Disable welcome message after SSH login](#disable-welcome-message-after-ssh-login)
  - [Set time zone](#set-time-zone)
  - [Set hostname](#set-hostname)
  - [See bash history](#see-bash-history)
  - [`ufw`](#ufw)

## VPS harderning

Set up aliases:

```bash
echo "alias cls='clear'" >> ~/.bashrc
# echo "alias ll='ls -la'" >> ~/.bashrc
```

Update packages:

```bash
sudo apt update && sudo apt upgrade -y
sudo apt install htop curl wget git vim -y
sudo apt autoremove -y
```

Fix "broken pipe" error by uncommenting `TCPKeepAlive yes` line:

```bash
vim /etc/ssh/sshd_config
```

```bash
sudo systemctl reboot
```

## Create a user and allow him to connect via SSH

Create a user and append this user to the group `sudo`:

```bash
sudo adduser myuser
# sudo userdel -r myuser
usermod -aG sudo myuser
```

Switch to a user:

```bash
su myuser
cd
```

Run this on the client:

```bash
ssh-copy-id -i ~/.ssh/key.pub myuser@remote_host
```

Test connection:

```bash
ssh bob@domain.xyz
```

## Change SSH port and disable password authentication

```bash
sudo vim /etc/ssh/sshd_config.d/50-cloud-init.conf
```

Add line:

```text
PasswordAuthentication no
Port 22000
```

Reboot:

```bash
sudo reboot
# Alternatively run:
#systemctl restart ssh
#systemctl restart sshd
```

When connecting to the server using the ssh command, you need to specify the port to connect using the `-p` flag:

```bash
ssh remote_username@remote_host -p SSH_PORT_NUMBER
```

## Watch temperature

### CPU

```bash
sudo apt install lm-sensors
yes yes | sudo sensors-detect
sudo service kmod start

watch sensors # see temperature values updating each second
```

### SSD

```bash
sudo apt update
wget http://archive.ubuntu.com/ubuntu/pool/universe/h/hddtemp/hddtemp_0.3-beta15-53_amd64.deb
sudo apt install ./hddtemp_0.3-beta15-53_amd64.deb

sudo watch hddtemp /dev/sda
```

## Enable/disable GUI for Ubuntu Desktop

```bash
sudo systemctl status gdm
sudo systemctl stop gdm
#sudo systemctl disable gdm
#sudo systemctl enable gdm
sudo systemctl start gdm
```

## Enable/disable Wi-Fi

```bash
nmcli radio wifi on
#nmcli radio wifi off
```

## User management

### Last logged in users

```bash
last
last USERNAME
last -n 5 # Limit the lines in the output
last -n 5 -R # Suppress where the user came from: the IP address, hostname, or a kernel version if it’s a system boot activity
```

The `last` command displays information about the last logged-in users. It's pretty convenient and handy when we need to track login activities or investigate a possible security breach.

The `last` command will, by default, take the system log file `/var/log/wtmp` as the data source to generate reports. The `wtmp` is a binary file on \*nix operating systems that maintains a history of all login and logout activities.

```bash
lastb
```

The `lastb` command is the same as the last command, except that, by default, it searches through the `/var/log/btmp` file, which contains all the bad login attempts. Regular users don't have read permission on the `/var/log/btmp` file:

```bash
ls -l /var/log/btmp
# -rw-rw---- 1 root utmp 1913472 Sep 13 00:06 /var/log/btmp
```

Therefore, only the `root` user can get the report of bad login attempts using the `lastb` command.

### List users

Find out if a user with name `bob` exists:

```bash
getent passwd | grep bob
```

Local user information is stored in the `/etc/passwd` file. Each line in this file represents login information for one user:

```bash
less /etc/passwd
```

Each line in the file has seven fields delimited by colons that contain the following information:

- Username
- Encrypted password; `x` means that the password is stored in the `/etc/shadow` file
- User ID number, UID
- User's group ID number, GID
- Full name of the user, GECOS
- User home directory
- Login shell, defaults to `/bin/bash`

### Elevate user's permissions

Elevate yourself to superuser:

```bash
sudo -i
```

Execute command and then run `exit` to log out to become an ordinary user:

```bash
cd /root
exit
```

### List all user groups

Show all existing groups:

```bash
cat /etc/group
```

Show group to which current user belongs:

```bash
groups
```

## Disable welcome message after SSH login

```sh
sudo chmod -x /etc/update-motd.d/*
```

## Set time zone

```sh
timedatectl
timedatectl list-timezones
sudo timedatectl set-timezone Europe/Moscow
date
```

## Set hostname

```bash
sudo hostnamectl set-hostname YOUR_HOSTNAME
hostnamectl
```

## See bash history

```bash
history
history 10
history | grep "search_term"
sudo vim /home/USER_YOU_WANT_TO_VIEW/.bash_history
```

## `ufw`

The `ss` (socket statistics) command is a modern utility that provides detailed information about network sockets:

```bash
sudo ss -tulnp
```

`t` — display TCP sockets; `u` — display UDP sockets; `l` — display listening sockets; `n` — do not resolve service names or hostnames (displays numerical port numbers and IP addresses); `p` — show the process name and PID.

Install Uncomplicated Firewall:

```bash
apt install ufw
ufw status
sudo ufw allow 22000/tcp
sudo less /etc/ufw/user.rules
# sudo ufw show added
sudo ufw default deny incoming
sudo ufw default allow outgoing
sudo ufw enable
```

From a second terminal/session (or your local machine), try to reconnect:

```bash
ssh -p 22000 user@your-server
```

Make sure you can log in. Only then should you close your original SSH session — this ensures you don't lock yourself out if something went wrong.

Check added rules:

```bash
sudo ufw status verbose
```

Upon installation, most applications that rely on network connections will register an application profile within UFW, which enables users to quickly allow or deny external access to a service. You can check which profiles are currently registered in UFW with:

```bash
sudo ufw app list
```

[↑ How to Set Up a Firewall with UFW on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu).
