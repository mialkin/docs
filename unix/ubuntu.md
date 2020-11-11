# Ubuntu

## Update packages

```sh
sudo apt update
sudo apt upgrade
sudo systemctl reboot
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

## Set up aliases

```bash
echo -e "alias ll='ls -la'\nalias cls='clear'" >> ~/.bashrc
```

## Install Nginx

```sh
sudo apt install nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```


