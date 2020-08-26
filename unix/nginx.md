# Nginx

## Configuration files

```sh
vim /etc/nginx/nginx.conf   # Default config file
cd /etc/nginx/conf.d        # Additional config files
```

## Reverse proxying

Create config file for a domain.

```sh
cd /etc/nginx/conf.d
sudo vim sub.domain.ru.conf
```

Example of a configuration file.

```text
server  {
  server_name sub.domain.ru;
  location / {
    proxy_pass http://localhost:5001;
  }
}
```

Restart Nginx.

```sh
sudo systemctl restart nginx
```

## HTML files

```sh
cd /var/www/
cd /usr/share/nginx/html/
```

## Mapping subdomain to folder

```text
server {
  server_name sub.domain.ru;
  location / {
    root /var/www/sub.domain.folder;
  }
}
```

## Permanent redirect from one page to another

```text
server {
  rewrite ^/old/url$ https://new.domain.ru/old/url permanent;
}

```

## See also

[Beginner’s Guide](http://nginx.org/en/docs/beginners_guide.html)
