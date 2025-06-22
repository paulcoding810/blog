---
date: "2024-09-27T23:17:56+07:00"
draft: false
title: "Self-hosting ntfy for Push Notifications"
summary: "A guide to setting up and using ntfy for self-hosted push notifications"
categories:
  - Code
tags:
  - ntfy
  - notifications
  - docker
  - caddy
---

## Introduction

ntfy is a simple, lightweight notification service that allows you to send push notifications to your devices. This guide will walk you through self-hosting ntfy and integrating it with other services.

## Common Issue: ntfy Not Working on HTTP

![ntfy requires https](./nttf-requires-https.png)

ntfy requires HTTPS for security reasons. We'll address this by using [Caddy as a reverse proxy](/posts/host-your-saas-with-caddy/).

## Hosting ntfy with Caddy

### Docker Compose Configuration

```yaml
networks:
  caddy_net:
    external: true
    name: "caddy_net"

services:
  ntfy:
    image: "binwiederhier/ntfy:latest"
    container_name: "ntfy"
    command: ["serve"]
    networks:
      - "caddy_net"
    ports:
      - "8080:80/tcp"
    restart: "unless-stopped"
    user: "1000:1000"
    volumes:
      - "./config:/etc/ntfy"
      - "./cache:/var/cache/ntfy"
      - "./lib:/var/lib/ntfy"
      - "/etc/timezone:/etc/timezone:ro"
      - "/etc/localtime:/etc/localtime:ro"
```

### Caddy Configuration

Add this to your Caddyfile:

```caddyfile
ntfy.your-domain {
    reverse_proxy ntfy:80
}
```

{{< notice warning >}}

**Note:** Set the port to 80, not 8080, as we're connecting to the `caddy_net`. Use 8080 for local testing only.

{{< /notice >}}

## Mobile Setup

1. Subscribe to a topic:
   ![subscribe to topic](./mobile-subscribe-topic.png)

2. Test the notification by `curl`:

   ```sh
   curl -d "Hello World" ntfy.yourdomain.com/admin
   ```

3. Or Push notification via ntfy dashboard:
   ![push notification](./push-notification.png)

4. Mobile notification result:
   ![mobile result](./notification-result.png)

## Integration Example: Radarr

1. Configure Radarr to use ntfy:
   ![config](radarr-ntfy.png)

2. Radarr notification result:
   ![result](radarr-ntfy-result.png)

## Auth

To enable authentication for your ntfy instance, follow these steps:

### Update Docker Compose File

Add the following environment variables to your `docker-compose.yml` file under the `ntfy` service:

```yaml
  environment:
    NTFY_BASE_URL: https://ntfy.paulcoding.com
    NTFY_CACHE_FILE: /var/lib/ntfy/cache.db
    NTFY_AUTH_FILE: /var/lib/ntfy/auth.db
    NTFY_AUTH_DEFAULT_ACCESS: deny-all
    NTFY_BEHIND_PROXY: true
    NTFY_ATTACHMENT_CACHE_DIR: /var/lib/ntfy/attachments
    NTFY_ENABLE_LOGIN: true
  volumes:
    - "./config:/etc/ntfy"
    - "./cache:/var/cache/ntfy"
    - "./lib:/var/lib/ntfy"
```

### Create a User

1. Access the ntfy container shell:

   ```sh
   docker exec -it ntfy sh
   ```

2. Add a new user with admin privileges:

   ```sh
   ntfy user add --role=admin paul
   ```

3. Generate a token for the user:

   ```sh
   ntfy token add paul
   ```

For more details, refer to the [ntfy documentation](https://docs.ntfy.sh/config/#example-private-instance).

## Conclusion

By self-hosting ntfy, you can have a secure, private notification system for various services. This setup using Docker and Caddy ensures HTTPS support and easy integration with other self-hosted applications.
