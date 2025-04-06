# Ubuntu

## Table of contents

- [Ubuntu](#ubuntu)
  - [Table of contents](#table-of-contents)
  - [Set up aliases](#set-up-aliases)
  - [Disable welcome message after SSH login](#disable-welcome-message-after-ssh-login)
  - [Update packages](#update-packages)
  - [Set time zone](#set-time-zone)
  - [Set hostname](#set-hostname)
  - [Grant user root priveleges](#grant-user-root-priveleges)
  - [Stop Ubuntu from asking for password](#stop-ubuntu-from-asking-for-password)
  - [Watch temperature](#watch-temperature)
    - [CPU](#cpu)
    - [SSD](#ssd)
  - [Enable/disable GUI for Ubuntu Desktop](#enabledisable-gui-for-ubuntu-desktop)
  - [Enable/disable Wi-Fi](#enabledisable-wi-fi)
  - [User management](#user-management)
    - [Create a user](#create-a-user)
    - [Switch to a user](#switch-to-a-user)
    - [List users](#list-users)
    - [Delete a user](#delete-a-user)
    - [Elevate user's permissions](#elevate-users-permissions)
    - [List all user groups](#list-all-user-groups)
    - [Add a password to a user](#add-a-password-to-a-user)
    - [Add a user to `sudo` group](#add-a-user-to-sudo-group)
    - [Allow a user to connect via SSH](#allow-a-user-to-connect-via-ssh)

## Set up aliases

```bash
echo "alias ll='ls -la'" >> ~/.bashrc && \
echo "alias cls='clear'" >> ~/.bashrc
```

## Disable welcome message after SSH login

```sh
sudo chmod -x /etc/update-motd.d/*
```

## Update packages

```sh
sudo apt update
sudo apt upgrade
sudo systemctl reboot
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

## Grant user root priveleges

```bash
usermod -aG sudo username
```

## Stop Ubuntu from asking for password

Change the line that says:

```bash
sudo vim /etc/sudoers
```

into:

```bash
%sudo  ALL=(ALL) NOPASSWD:ALL
```

or even:

```bash
%sudo  ALL=(ALL:ALL) NOPASSWD:ALL
```

Restart session.

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

### Create a user

Create a new user `sammy`:

```bash
sudo adduser sammy --disabled-password
```

The `--disabled-password` option will not set a password, meaning no password is legal, but login is still possible, for example with SSH RSA keys.

### Switch to a user

Switch to a user `sammy` that was created without password:

```bash
sudo -u sammy -s
```

Exit:

```bash
exit
```

### List users

Find out if a user with name `sammy` exists:

```bash
getent passwd | grep sammy
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

### Delete a user

Delete user with its data including `/home/sammy` folder:

```bash
sudo userdel -r sammy
```

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

### Add a password to a user

```bash
sudo passwd sammy
```

### Add a user to `sudo` group

```bash
usermod -aG sudo sammy
```

### Allow a user to connect via SSH

Being logged in as a new user create new `.ssh` directory:

```bash
mkdir ~/.ssh
```

Copy your public key to `authorized_keys` file:

```bash
echo public_key_string >> ~/.ssh/authorized_keys
```

Recursively remove all "group" and "other" permissions for the ~/.ssh/ directory:

```bash
chmod -R go= ~/.ssh
```

If you're using the root account to set up keys for a user account, it's also important that the `~/.ssh` directory belongs to the user and not to root:

```bash
chown -R sammy:sammy ~/.ssh
```

Test connection:

```bash
ssh sammy@domain.xyz
```
