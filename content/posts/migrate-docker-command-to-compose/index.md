---
date: "2025-06-14T21:51:22+07:00"
draft: false
title: "Switching from Docker Commands to Docker Compose"
summary: "See how Docker Compose can simplify your Docker workflow in just a few steps."
categories:
  - Code
tags:
  - docker
  - docker-compose
---

## Make Your Life Easier with Docker Compose

Tired of running long Docker commands? Try Docker Compose! With a simple YAML file, you can manage everything at once. It's also better for backup purpose. Here’s how to switch:

---

### 1. Auto-Generate Your Compose File

Use [docker-autocompose](https://github.com/Red5d/docker-autocompose) to scan your running containers and create a `docker-compose.yml` for you.

---

### 2. Clean Up Old Containers

Stop and remove old containers:

```bash
docker container ls -a | grep container_name
docker container stop container_id
docker container rm container_id
```

---

### 3. Start with Compose

Spin everything up easily:

```bash
docker compose up -d
```

---

### 4. Move Your Data (If Needed)

If you have data folders, copy them to your new setup and set permissions:

```bash
sudo rsync -aP data_dir/ new_compose_dir/data_dir/
sudo chown user:group -R new_compose_dir/data_dir/
```

Update the paths in your `docker-compose.yml` if necessary.

---

That’s it—enjoy an easier, tidier Docker workflow!
