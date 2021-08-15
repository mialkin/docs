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

## Install Nginx

```sh
sudo apt install -y nginx
sudo systemctl enable nginx
sudo systemctl start nginx
```

[Configure Nginx](nginx.md) once the installation is finished.

## Temperature & frequency (Raspberry Pi)

Temperature:

```bash
paste <(cat /sys/class/thermal/thermal_zone*/type) <(cat /sys/class/thermal/thermal_zone*/temp) | column -s $"\t" -t | sed "s/\(.\)..$/.\1°C/"
```

Frequency:

```bash
echo ">> CPU Freq:" $(($(cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_cur_freq)/1000))MHz
```
