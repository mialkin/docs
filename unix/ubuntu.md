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

## Set up aliases

```bash
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
wget http://archive.ubuntu.com/ubuntu/pool/universe/h/hddtemp/hddtemp_0.3-beta15-54_amd64.deb  
sudo apt install hddtemp

sudo watch hddtemp /dev/sda
```
