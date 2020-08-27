# Ubuntu

## Update packages

```sh
sudo apt update
sudo apt upgrade
sudo systemctl reboot
```

## Install Nginx

```sh
sudo apt install nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```

## Set time zone

```sh
timedatectl
timedatectl list-timezones
sudo timedatectl set-timezone Europe/Moscow
date
```
