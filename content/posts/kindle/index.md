---
date: '2025-01-22T09:39:11+07:00'
draft: false
title: 'Manage your Kindle+Koreader'
summary: 'Essential commands and tips for managing your Kindle'
categories:
- Code
tags:
- ssh
- ereader
- kindle
---

This guide covers essential commands and configurations for managing your Kindle device with KOReader installed.

## Basic Device Access

### SSH Connection

To establish a secure shell connection to your Kindle:

```bash
ssh root@192.168.0.17 -p 2222
```

Make sure your Kindle is connected to the same network as your computer before attempting the connection.

## File Management

### Dictionary Installation

Transfer dictionary files to your Kindle using SCP:

```bash
scp -P 2222 ~/Downloads/dict-en-en/* root@192.168.0.17:/mnt/us/koreader/data/dict
```

You can find pre-compiled dictionaries at the [BoboTiG/ebook-reader-dict repository](https://github.com/BoboTiG/ebook-reader-dict).

## System Configuration

### File Server Setup

Disable authentication on the built-in file server for easier access:

```bash
cd /mnt/us/extensions/filebrowser
fileserver config set --auth.method=noauth
```

### Remove Advertisements

To eliminate ads from your Kindle's lock screen:

```bash
rm -rf /mnt/us/system/.assets
```

**Note**: This operation cannot be undone. Make sure you want to remove the ads permanently.

## Additional Resources

For more detailed information, consult these resources:

- [KOReader Documentation](https://github.com/koreader/koreader/wiki/Dictionary-support) - Official wiki for KOReader
- [FileServer Configuration Guide](https://filebrowser.org/configuration/authentication-method) - Detailed file server setup instructions

## Tips

- Always backup your device before making system-level changes
- Keep track of your device's IP address for SSH connections
- Regularly update your dictionaries for better reading experience
