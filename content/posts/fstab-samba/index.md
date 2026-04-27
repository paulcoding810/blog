---
date: "2026-04-27T08:50:51+07:00"
draft: false
title: "Mounting Samba Shares via /etc/fstab"
summary: "How to persistently mount a Samba share from another machine using /etc/fstab on Linux."
categories:
  - Code
tags:
  - linux
  - samba
  - fstab
  - mount
---

{{< gpt >}}

Mounting a Samba share from another machine is a common need — whether it's a NAS, another Linux server, or a dedicated storage box. Here's the clean way to do it persistently.

---

## Create a mount point

```bash
sudo mkdir -p /mnt/immich
```

---

## Create a credentials file

Create a file to store your Samba credentials securely:

```bash
sudo nano /root/.smbcredentials
```

```ini
username=paul
password=YOUR_PASSWORD
```

Secure it so only root can read it:

```bash
sudo chmod 600 /root/.smbcredentials
```

---

## Add to `/etc/fstab`

```fstab
//SERVER_IP/immich  /mnt/immich  cifs  credentials=/root/.smbcredentials,iocharset=utf8,uid=1000,gid=1000  0  0
```

Replace:

- `SERVER_IP` — your Samba server address
- `uid`/`gid` — the local user who should own the mounted files

Then mount it:

```bash
sudo mount -a
```

No output means success.

---

## Common fixes

### Install CIFS tools

If mounting fails, you may need the CIFS utilities:

```bash
sudo apt install cifs-utils
```

### Specify SMB version

Older servers need a specific SMB version:

```fstab
...,vers=3.1.1
```

For very old servers:

```fstab
...,vers=3.0
```

---

## A solid example

```fstab
//192.168.1.10/immich /mnt/immich cifs credentials=/root/.smbcredentials,vers=3.1.1,uid=1000,gid=1000,_netdev 0 0
```

The `_netdev` option tells the system to wait until the network is up before mounting — prevents boot hangs on headless servers.

---

## Permissions

- `uid`/`gid` controls **local ownership** of files
- Samba server permissions still apply based on the server config
- The interaction between `create mask` on the server and `uid`/`gid` on the client determines the final behavior

---

If you're mounting from a different OS — another Linux server, macOS, or Windows — let me know and I can give you the exact approach for that.
