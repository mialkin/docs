# Ubuntu

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
