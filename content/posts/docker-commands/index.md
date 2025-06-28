---
date: "2025-01-22T14:12:36+07:00"
draft: false
title: "Essential Docker Commands for Daily Use"
summary: "A comprehensive guide to commonly used Docker and Docker Compose commands"
categories:
  - Development
  - DevOps
tags:
  - docker
  - docker-compose
  - containers
  - cli
---

## Basic Docker Compose Commands

### Starting Containers

```bash
# Start containers in detached mode
docker compose up -d

# Start specific service
docker compose up -d service_name
```

### Stopping Containers

```bash
# Stop and remove containers
docker compose down

# Stop and remove containers, including volumes
docker compose down -v
```

### Viewing Logs

```bash
# Follow logs in real-time
docker compose logs -f

# Follow logs for specific service
docker compose logs -f service_name
```

### Accessing Containers

```bash
# Access container shell
docker compose exec container_name bash

# Run one-off command
docker compose exec container_name command
```

### Copy Files from Containers

```bash
docker cp container_id:/path/to/file ./destination
```

### Container Management

```bash
# List running containers
docker compose ps

# Restart services
docker compose restart

# View resource usage
docker compose top

# View container stats
docker stats --no-stream

# Sort containers by resource usage, by memory descending
docker stats --no-stream | sort -k 4 -h -r

# Build or rebuild services
docker compose build

# Build with no cache
docker compose build --no-cache

### Validate Compose Configuration
docker compose config
```

## Best Practices

1. Always use `-d` flag for detached mode in production
2. Regularly check logs for issues
3. Clean up unused containers and volumes
4. Use meaningful container names
5. Keep your Docker Compose files version controlled
