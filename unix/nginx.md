# Nginx

## Configuration files

```sh
vim /etc/nginx/nginx.conf   # Default config file
cd /etc/nginx/conf.d        # Additional config files
```

## Static website

Create config file for a domain.

```sh
cd /etc/nginx/conf.d
sudo vim sub.domain.ru.conf
```

Example of a config file.

```text
server {
  server_name sub.domain.ru;
  location / {
    root /var/www/sub.domain.folder;
  }
}
```

Test config files and restart Nginx.

```sh
sudo nginx -t
sudo systemctl restart nginx
```

## Dynamic website (reverse proxy)

Create config file for a domain.

```sh
cd /etc/nginx/conf.d
sudo vim sub.domain.ru.conf
```

Example of a config file.

```text
server  {
  server_name sub.domain.ru;
  location / {
    proxy_pass http://localhost:5000;
  }
}
```

Test config files and restart Nginx.

```sh
sudo nginx -t
sudo systemctl restart nginx
```

[↑ NGINX Reverse Proxy](https://docs.nginx.com/nginx/admin-guide/web-server/reverse-proxy/)

## HTML files

```sh
cd /var/www/
cd /usr/share/nginx/html/
```

## Load balancing

[↑ NGINX as a load balancer](https://www.youtube.com/watch?v=v81CzSeiQjo)

[↑ HTTP Load Balancing](https://docs.nginx.com/nginx/admin-guide/load-balancer/http-load-balancer/)

## Permanent redirect from one page to another

```text
server {
  rewrite ^/old/url$ https://new.domain.ru/old/url permanent;
}

```

## See also

[↑ Beginner's Guide](http://nginx.org/en/docs/beginners_guide.html)
