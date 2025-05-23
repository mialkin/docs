# Ubuntu

## Table of contents

- [Ubuntu](#ubuntu)
  - [Table of contents](#table-of-contents)
  - [Set up aliases](#set-up-aliases)
  - [Disable welcome message after SSH login](#disable-welcome-message-after-ssh-login)
  - [Update packages](#update-packages)
  - [Set time zone](#set-time-zone)
  - [Set hostname](#set-hostname)
  - [Create a user](#create-a-user)
  - [Grant user root priveleges](#grant-user-root-priveleges)
  - [Change SSH port](#change-ssh-port)
  - [Add a password to a user](#add-a-password-to-a-user)
  - [Switch to a user](#switch-to-a-user)
  - [Allow a user to connect via SSH](#allow-a-user-to-connect-via-ssh)
  - [Disable password authentication](#disable-password-authentication)
  - [Delete a user](#delete-a-user)
  - [Watch temperature](#watch-temperature)
    - [CPU](#cpu)
    - [SSD](#ssd)
  - [Enable/disable GUI for Ubuntu Desktop](#enabledisable-gui-for-ubuntu-desktop)
  - [Enable/disable Wi-Fi](#enabledisable-wi-fi)
  - [User management](#user-management)
    - [List users](#list-users)
    - [Elevate user's permissions](#elevate-users-permissions)
    - [List all user groups](#list-all-user-groups)

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

## Create a user

```bash
sudo adduser bob --disabled-password
```

The `--disabled-password` option will not set a password, meaning no password is legal, but login is still possible, for example with SSH RSA keys.

## Grant user root priveleges

```bash
usermod -aG sudo bob
```

## Change SSH port

```bash
sudo vim /etc/ssh/sshd_config.d/50-cloud-init.conf
```

Add line:

```text
Port 22000
```

Restart the SSH server:

```bash
systemctl restart sshd
# sudo systemctl restart ssh
```

When connecting to the server using the ssh command, you need to specify the port to connect using the `-p` flag:

```bash
ssh remote_username@remote_host -p SSH_PORT_NUMBER
```

## Add a password to a user

```bash
sudo passwd bob
```

## Switch to a user

```bash
su bob
```

## Allow a user to connect via SSH

Being logged in as a new user create new `.ssh` directory:

```bash
mkdir ~/.ssh
```

Copy your public key to `authorized_keys` file:

```bash
echo "PUBLIC_KEY_STRING" >> ~/.ssh/authorized_keys
# Or run this on the client:
# ssh-copy-id -i ~/.ssh/id_rsa.pub remote_username@remote_host
```

Recursively remove all "group" and "other" permissions for the ~/.ssh/ directory:

```bash
chmod -R go= ~/.ssh
```

If you're using the root account to set up keys for a user account, it's also important that the `~/.ssh` directory belongs to the user and not to root:

```bash
chown -R bob:bob ~/.ssh
```

Test connection:

```bash
ssh bob@domain.xyz
```

## Disable password authentication

```bash
sudo vim /etc/ssh/sshd_config.d/50-cloud-init.conf
```

Replace:

```text
PasswordAuthentication yes
```

with

```text
PasswordAuthentication no
```

Restart the SSH server:

```bash
sudo systemctl restart sshd
# sudo systemctl restart ssh
```

## Delete a user

Delete user with its data including `/home/bob` folder:

```bash
sudo userdel -r bob
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
