# VPN

## Table of contents

- [VPN](#vpn)
  - [Table of contents](#table-of-contents)
  - [Xray-Core](#xray-core)
    - [Key Features](#key-features)
    - [Use cases](#use-cases)
    - [3X-UI](#3x-ui)
    - [Clients](#clients)

## Xray-Core

[↑ Xray-core](https://github.com/XTLS/Xray-core) is the core component of the Xray platform, which is an open-source network proxy tool primarily used for circumventing internet censorship, enhancing online privacy, and securing internet connections.

### Key Features

**Multiple Protocols**:

- VMess (like in V2Ray)
- VLESS (a lightweight and improved version of VMess)
- Trojan (TLS-based protocol)
- Shadowsocks
- SOCKS, HTTP, etc

**Built-in obfuscation**:

- Makes traffic less detectable by DPI (Deep Packet Inspection)
- Can disguise traffic as HTTPS, making it look like regular encrypted web traffic

**Multiplexing & routing**:

- Smart routing based on domain, IP, or protocol
- Load balancing and fallback options

**Encryption & TLS support**:

Supports TLS 1.3 and XTLS (a more efficient TLS transport used mainly with VLESS).

**Cross-platform & lightweight**:

- Can run on Linux, Windows, macOS, and more
- Often used in conjunction with tools like Nginx, Caddy, or Cloudflare for extra obfuscation.

### Use cases

- Bypass geo-restrictions and firewalls (e.g., the Great Firewall of China)
- Secure communication over public networks (like public Wi-Fi)
- Hosting private VPN/proxy servers

### 3X-UI

[↑ 3X-UI](https://github.com/MHSanaei/3x-ui) is an open-source graphical management panel designed for configuring and managing Xray-core proxy servers. It provides a user-friendly web interface that simplifies the setup and administration of various proxy protocols.

Nginx reverse proxy configuration:

```bash
server {
  listen 443 ssl;
  server_name your.site.com;

  ssl_certificate /etc/letsencrypt/live/your.site.com/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/your.site.com/privkey.pem;

  location / {
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_set_header X-Forwarded-Proto $scheme;
      proxy_set_header Host $http_host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header Range $http_range;
      proxy_set_header If-Range $http_if_range;
      proxy_redirect off;
      proxy_pass http://127.0.0.1:2053;
  }
}
```

```bash
cd /etc/nginx/sites-enabled
sudo unlink default
sudo ln -s /etc/nginx/sites-available/3x-ui 3x-ui
```

If you forgot your login info, you can type 'x-ui settings' to check.

```text
┌───────────────────────────────────────────────────────┐
│  x-ui control menu usages (subcommands):              │
│                                                       │
│  x-ui              - Admin Management Script          │
│  x-ui start        - Start                            │
│  x-ui stop         - Stop                             │
│  x-ui restart      - Restart                          │
│  x-ui status       - Current Status                   │
│  x-ui settings     - Current Settings                 │
│  x-ui enable       - Enable Autostart on OS Startup   │
│  x-ui disable      - Disable Autostart on OS Startup  │
│  x-ui log          - Check logs                       │
│  x-ui banlog       - Check Fail2ban ban logs          │
│  x-ui update       - Update                           │
│  x-ui legacy       - legacy version                   │
│  x-ui install      - Install                          │
│  x-ui uninstall    - Uninstall                        │
└───────────────────────────────────────────────────────┘
```

### Clients

[↑ v2RayTun](https://v2raytun.com) is an Xray Core-based client.
