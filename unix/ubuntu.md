# Ubuntu

## Table of contents

<!-- TOC -->
* [Ubuntu](#ubuntu)
  * [Table of contents](#table-of-contents)
  * [VPS hardening](#vps-hardening)
  * [Change SSH port and disable password authentication](#change-ssh-port-and-disable-password-authentication)
  * [`ufw`](#ufw)
    * [Disable port](#disable-port)
  * [User management](#user-management)
    * [Last logged-in users](#last-logged-in-users)
    * [List users](#list-users)
    * [Elevate user's permissions](#elevate-users-permissions)
    * [List all user groups](#list-all-user-groups)
  * [See bash history](#see-bash-history)
  * [`ss`](#ss)
<!-- TOC -->

## VPS hardening

```bash
NEW_USER="bob"; \
apt-get update && apt-get upgrade -y && \
apt-get install -y htop curl wget vim ufw && \
apt-get autoremove -y && \
# Disable a welcome message after SSH login
chmod -x /etc/update-motd.d/* && \
# Add a new SSH user
adduser "$NEW_USER" && \
usermod -aG sudo "$NEW_USER"
```

Uncomment `TCPKeepAlive yes` line to prevent "broken pipe" error:

```bash
vim /etc/ssh/sshd_config
```

Reboot:

```bash
systemctl reboot
```

Run this on your local computer to take your local public SSH key `key.pub` and copy it over to a remote's `~/.ssh/authorized_keys` file. It will prompt you for Bob's password (the one you set during the setup script):

```bash
ssh-copy-id -i ~/.ssh/key.pub bob@your_server_ip_or_name
# Verify it's working by connecting using Bob's private key
ssh -i ~/.ssh/key bob@your_server_ip_or_name
```

```bash
echo "alias cls='clear'" >> ~/.bashrc && \
source ~/.bashrc
sudo hostnamectl set-hostname YOUR_HOSTNAME
hostnamectl
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
sudo systemctl reboot
```

When connecting to the server using the ssh command, you need to specify the port to connect using the `-p` flag:

```bash
ssh bob@your_server_ip_or_name -p SSH_PORT_NUMBER
```

Set time zone:

```bash
timedatectl && \
sudo timedatectl set-timezone Europe/Moscow && \
date
```

## `ufw`

```bash
# apt install ufw
# Switch to the root user to avoid typing in sudo in front of each command
# sudo -i
ufw status
ufw status
ufw allow 22000/tcp
# ufw allow 80/tcp
# ufw allow 443/tcp
less /etc/ufw/user.rules
# ufw show added
ufw default deny incoming
ufw default allow outgoing
ufw enable
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

Allow Nginx:

```bash
sudo ufw allow 'Nginx Full'
```

[↑ How to Set Up a Firewall with UFW on Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-a-firewall-with-ufw-on-ubuntu).

### Disable port

```bash
sudo ufw deny 12345/tcp
sudo ufw status numbered
sudo ufw delete 4
```

## User management

### Last logged-in users

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

Show the group to which the current user belongs:

```bash
groups
```

## See bash history

```bash
history
history 10
history | grep "search_term"
sudo vim /home/USER_YOU_WANT_TO_VIEW/.bash_history
```

## `ss`

The `ss` (socket statistics) command is a modern utility that provides detailed information about network sockets:

```bash
sudo ss -tulnp
```

`t` — display TCP sockets; `u` — display UDP sockets; `l` — display listening sockets; `n` — do not resolve service names or hostnames (displays numerical port numbers and IP addresses); `p` — show the process name and PID.
