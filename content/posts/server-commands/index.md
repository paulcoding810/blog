---
date: "2025-05-31T21:20:00+07:00"
draft: false
title: "Server Commands"
summary: "A collection of useful server commands for system monitoring and management."
categories:
  - Code
  - Blog
tags:
  - ssh
  - server
  - commands
---

### Useful Server Commands

#### Check Disk Space

To check the available disk space on your system, use the following command:

```bash
df -h
```

#### Check Directory Size

To check the size of directories in the current path, sorted by size:

```bash
du -h -d 1 | sort -hr
```

#### Check GPU Information

To list GPU-related information on your system:

```bash
lspci | grep -i vga
```
