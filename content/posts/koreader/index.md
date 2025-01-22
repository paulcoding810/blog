---
date: '2025-01-22T09:39:11+07:00'
draft: false
title: 'Manage your KOReader'
summary: 'Essential commands and tips for managing your KOReader device'
categories:
- Code
tags:
- ssh
- ereader
- kindle
---

## Introduction

KOReader is a document viewer primarily aimed at e-ink devices. This guide covers useful commands and tips for managing your KOReader installation, including SSH access and file management.

## Essential Commands

### SSH Connection

To connect to your device via SSH:

```bash
ssh root@192.168.0.17
```

### File Transfer

Copy dictionary files to KOReader using SCP:

```bash
scp -P 2222 ~/Downloads/dict-en-en/* root@192.168.0.17:/mnt/us/koreader/data/dict
```

[Dictionaries download link](https://github.com/BoboTiG/ebook-reader-dict)

### File Server Configuration

To disable authentication on the built-in file server:

```bash
cd /mnt/us/extensions/filebrowser
fileserver config set --auth.method=noauth
```

## Tips and Best Practices

1. Always ensure your device is on the same network before attempting SSH connections
2. Back up your KOReader configuration files regularly
3. Keep track of your device's IP address

## Useful Resources

- [KOReader Documentation](https://github.com/koreader/koreader/wiki/Dictionary-support)
- [FileServer Configuration Guide](https://filebrowser.org/configuration/authentication-method)

## Troubleshooting

Common issues and their solutions:
- SSH connection refused
- File transfer errors
- Permission problems
