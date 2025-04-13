# Nginx

[↑ Nginx](https://nginx.org) ("engine x") is an HTTP web server, reverse proxy, content cache, load balancer, TCP/UDP proxy server, and mail proxy server.

## Table of context

- [Nginx](#nginx)
  - [Table of context](#table-of-context)
  - [Installation](#installation)
  - [Configuration files](#configuration-files)
  - [Static website](#static-website)
  - [Reverse proxy](#reverse-proxy)
  - [HTML files](#html-files)
  - [Permanent redirect from one page to another](#permanent-redirect-from-one-page-to-another)

## Installation

```bash
sudo apt update
sudo apt install nginx
```

## Configuration files

```sh
vim /etc/nginx/nginx.conf   # Default config file
cd /etc/nginx/conf.d        # Additional config files
```

Default website:

```bash
cd /etc/nginx/sites-enabled
vim /etc/nginx/sites-available/default
# Unlink default website: unlink default
# Link website: sudo ln -s /etc/nginx/sites-available/your-site your-site
```

Default website files:

```bash
cd /var/www/html
```

## Static website

Create config file for a domain:

```sh
cd /etc/nginx/conf.d
sudo vim sub.domain.ru.conf
```

Example of a config file:

```text
server {
  server_name sub.domain.ru;
  location / {
    root /var/www/sub.domain.folder;
  }
}
```

Test config files and restart Nginx:

```sh
sudo nginx -t
sudo systemctl restart nginx
```

## Reverse proxy

Create config file for a domain:

```sh
cd /etc/nginx/conf.d
sudo vim sub.domain.ru.conf
```

Example of a config file:

```text
server  {
  server_name sub.domain.ru;
  location / {
    proxy_pass http://localhost:5000;
  }
}
```

Test config files and restart Nginx:

```sh
sudo nginx -t
sudo systemctl restart nginx
```

[↑ NGINX Reverse Proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/).

## HTML files

```sh
cd /var/www/
cd /usr/share/nginx/html/
```

## Permanent redirect from one page to another

```text
server {
  rewrite ^/old/url$ https://new.domain.ru/old/url permanent;
}
```
