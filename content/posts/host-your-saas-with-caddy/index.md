---
date: '2024-09-18T23:17:41+07:00'
draft: false
title: 'Host Your SaaS with Caddy'
summary: 'A comprehensive guide to hosting your services securely using Caddy as a reverse proxy'
categories:
- DevOps
tags:
- caddy
- docker
- cloudflare
- reverse-proxy
---

## Table of Contents

- [Table of Contents](#table-of-contents)
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [1. Setting up Caddy with Docker](#1-setting-up-caddy-with-docker)
  - [Docker Compose Configuration](#docker-compose-configuration)
  - [Dockerfile for Caddy](#dockerfile-for-caddy)
  - [Environment Variables](#environment-variables)
  - [Caddy Configuration](#caddy-configuration)
- [2. Configuring Services](#2-configuring-services)
- [3. Testing Your Setup](#3-testing-your-setup)
- [Troubleshooting](#troubleshooting)
- [Conclusion](#conclusion)
- [Next Steps](#next-steps)
- [References](#references)

## Introduction

[Caddy](https://caddyserver.com/docs/) is a powerful, enterprise-ready, open source web server with automatic HTTPS written in Go. It's known for its simplicity and ease of use, making it an excellent choice for hosting SaaS applications. This guide will walk you through setting up a secure hosting environment for your services using Caddy as a reverse proxy.

**Key benefits of using Caddy include:**

- Automatic HTTPS with Let's Encrypt
- Easy configuration with the Caddyfile
- Built-in support for various plugins
- High performance and security

This post will cover:

1. Setting up Caddy with Docker
2. Configuring services to work with Caddy
3. Testing and troubleshooting your setup

## Prerequisites

Before we begin, ensure you have:

- Basic understanding of Docker and Docker Compose
- A domain name
- Cloudflare account for DNS management
- Docker and Docker Compose installed on your host machine
- Basic command-line knowledge

## 1. Setting up Caddy with Docker

Let's set up Caddy as our reverse proxy using Docker.

### Docker Compose Configuration

Create a new `docker-compose.yml` file for Caddy:

```yaml
services:
  caddy:
    build: ./dockerfile-caddy
    container_name: caddy
    hostname: caddy
    restart: unless-stopped
    env_file: .env
    ports:
      - "80:80"
      - "443:443"
      - "443:443/udp"
    volumes:
      - ./Caddyfile:/etc/caddy/Caddyfile
      - ./caddy_config:/config
      - ./caddy_data:/data

networks:
  default:
    name: $DOCKER_MY_NETWORK
    external: true
```

This configuration sets up Caddy with the necessary port mappings and volume mounts.

### Dockerfile for Caddy

Create a `./dockerfile-caddy/Dockerfile` with:

```dockerfile
FROM caddy:2.8.4-builder AS builder

RUN xcaddy build \
    # ssl things
    --with github.com/caddy-dns/cloudflare \
    # ddns things
    --with github.com/mholt/caddy-dynamicdns

FROM caddy:2.8.4

COPY --from=builder /usr/bin/caddy /usr/bin/caddy
```

This Dockerfile builds Caddy with additional plugins for Cloudflare DNS and Dynamic DNS support.

### Environment Variables

Create a `.env` file:

```dotenv
TZ=Asia/Ho_Chi_Minh
DOCKER_MY_NETWORK=caddy_net
MY_DOMAIN=paulcoding.com
CLOUDFLARE_API_TOKEN=your-cf-token-here
```

Adjust these variables according to your setup.

### Caddy Configuration

Create a `Caddyfile`:

```caddyfile
(LAN_only) {
    @fuck_off_world {
        not remote_ip 192.168.1.0/24
    }
    respond @fuck_off_world "NOT FUCKING FOUND" 403
}

{
  # ssl shit
  acme_dns cloudflare {$CLOUDFLARE_API_TOKEN}
  # ddns shit
  dynamic_dns {
    provider cloudflare {$CLOUDFLARE_API_TOKEN}
    domains {
      paulcoding.com home git download
    }
  }
}

home.{$MY_DOMAIN} {
    reverse_proxy glances:61208
    import LAN_only
}

download.{$MY_DOMAIN} {
    reverse_proxy jdownloader:5800
    import LAN_only
}

speed.{$MY_DOMAIN} {
    reverse_proxy speedtest:8001
}
```

Explanation of key components:

- `LAN_only`: A snippet that restricts access to local network only
- `acme_dns`: Configures SSL using Cloudflare DNS challenge
- `dynamic_dns`: Keeps your domain IP up to date
- Service blocks: Define reverse proxy rules for [services you're hosting](/posts/my-self-hosted/)

{{< collapse summary="You can consider showing [some cats](https://github.com/caddyserver/caddy/issues/3336#issuecomment-638476670) for errors instead" >}}

```caddyfile
handle_errors {
    rewrite * /{http.error.status_code}
    reverse_proxy https://http.cat
}
```

{{< /collapse >}}

Deploy Caddy:

```sh
docker compose up -d
```

To reload Caddy's configuration:

```sh
docker exec -w /etc/caddy caddy caddy reload
```

Check logs for debugging:

```sh
docker logs caddy
```

## 2. Configuring Services

Ensure all your services are on the same Docker network as Caddy (`caddy_net`):

![Caddy network](./container-same-network.png)

Add your services to the same network in their respective Docker Compose files or when running the containers.

## 3. Testing Your Setup

After configuration, you should be able to access your services via their respective subdomains:

| Subdomain   | Screenshot |
|-----------  |------------|
| [download.paulcoding.com](https://download.paulcoding.com)  | ![JDownloader](./subdomain-download.png) |
| [home.paulcoding.com](https://home.paulcoding.com)  | ![Glances](./subdomain-home.png) |
| [speed.paulcoding.com](https://speed.paulcoding.com)  | ![Speedtest](./subdomain-speed.png) |

If not on the LAN, you'll see an access denied message:

{{< icon src="./not-in-lan.png" alt="Pi-hole" width="300"  >}}

## Troubleshooting

1. **SSL Certificate Issues**
   - Ensure your Cloudflare API token has the correct permissions
   - Check Caddy logs for specific SSL-related errors

2. **Service Unreachable**
   - Verify that the service is running and on the correct Docker network
   - Check if the port in the Caddyfile matches the service's exposed port

3. **Configuration Not Updating**
   - After changing the Caddyfile, remember to reload Caddy
   - If changes don't take effect, try restarting the Caddy container

4. **LAN-only Access Not Working**
   - Verify your local network's IP range in the `LAN_only` snippet
   - Ensure the `import LAN_only` directive is correctly placed in each service block

## Conclusion

You've now set up a secure hosting environment for your services using Caddy as a reverse proxy. This setup provides SSL encryption, easy service management, and LAN-only access for specific services. Caddy's simplicity and power make it an excellent choice for hosting SaaS applications, providing both security and flexibility.

## Next Steps

To further enhance your setup, consider:

- Implementing additional security measures like [fail2ban](https://github.com/fail2ban/fail2ban)
- Setting up monitoring and alerting for your services
- Exploring more advanced Caddy features and plugins

## References

- [Caddy DNS Challenge Guide](https://github.com/DoTheEvo/selfhosted-apps-docker/tree/master/caddy_v2#Caddy-DNS-challenge)
