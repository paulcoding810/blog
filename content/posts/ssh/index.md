---
date: "2025-06-15T10:31:38+07:00"
draft: false
title: "SSH Guide"
summary: "A comprehensive guide to installing, configuring, and troubleshooting SSH."
categories:
  - Code
tags:
  - ssh
  - troubleshooting
---

## Installing OpenSSH Server

To install the OpenSSH server, run the following command:

```bash
sudo apt install openssh-server
```

## SSH Configuration Files

- **Server Configuration**: `/etc/ssh/sshd_config`
- **Client Configuration**: `/etc/ssh/ssh_config`

## Managing SSH with `systemctl`

Use the following commands to manage the SSH service:

```bash
sudo systemctl start sshd       # Start the SSH service
sudo systemctl status sshd      # Check the status of the SSH service
sudo systemctl stop sshd        # Stop the SSH service
sudo systemctl restart sshd     # Restart the SSH service
sudo systemctl enable sshd      # Enable SSH to start on boot
sudo systemctl disable sshd     # Disable SSH from starting on boot
```

## Troubleshooting Common Issues

### Permission Denied (root@host)

If you encounter a "Permission denied" error when trying to log in as root, ensure that root login is permitted in the `/etc/ssh/sshd_config` file. Update the configuration as follows:

```conf
PermitRootLogin yes
```

After making changes, restart the SSH service:

```bash
sudo systemctl restart sshd
```

## Extra Resources

- [Quick connection with ssh aliases](../ssh-alias/)