# Caddy The Ultimate Server with plugins

![Docker Image Version](https://img.shields.io/docker/stars/zotta/caddy-plugins) ![Docker Pulls](https://img.shields.io/docker/pulls/zotta/caddy-plugins) ![Docker Image Version](https://img.shields.io/docker/v/zotta/caddy-plugins)

Custom Docker images of Caddy, built with multi-architecture support (`linux/amd64` and `linux/arm64`) and integrated with plugins.
Designed for advanced environments, secure proxies, and automated deployments.

---

## Features

- Based on the official Caddy (latest stable version)
- Includes plugins
- Multi-architecture support:
  - `linux/amd64`
  - `linux/arm64`

---

## Supported tags and respective `Dockerfile` links
- [`2.11.2-plugins2.1`, `2.11.2-plugins`, `2.11-plugins`, `latest`](https://github.com/Frikilinux/docker-images/blob/main/caddy-plugins/Dockerfile)

## Included Plugins

| Plugin | Description | Repository | Commit |
|--------|-------------|------------|--------|
| `tls.dns.cloudflare` | Enables DNS-01 authentication with Cloudflare's API | [caddy-dns/cloudflare](https://github.com/caddy-dns/cloudflare) | [6dc1fbb](https://github.com/caddy-dns/cloudflare/tree/6dc1fbb7e925b0da6736780db2e90f4816b79ab7) |
| `layer4` | Adds TCP/UDP proxying and advanced connection routing | [mholt/caddy-l4](https://github.com/mholt/caddy-l4) | [1a3490e](https://github.com/mholt/caddy-l4/tree/1a3490ef786a8fd93128d55aed1999930b7c0586) |

---

## Basic Usage

```bash
$ docker run -d --cap-add=NET_ADMIN -p 80:80 -p 443:443 -p 443:443/udp \
    -v ./conf:/etc/caddy \
    -v ./site:/srv \
    -v caddy_data:/data \
    -v caddy_config:/config \
    zotta/caddy-plugins:latest
```

## Docker compose

Example `compose.yaml` file:

```yaml
services:
  caddy:
    # Replace <version> with the desired version tag, e.g., latest
    image: zotta/caddy-plugins:<version>
    restart: unless-stopped
    cap_add:
      - NET_ADMIN
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - $PWD/conf:/etc/caddy
      - $PWD/site:/srv
      - caddy_data:/data
      - caddy_config:/config

volumes:
  caddy_data:
  caddy_config:

```

See [Caddy Documentation](https://caddyserver.com/docs/) for more configuration options and details.
