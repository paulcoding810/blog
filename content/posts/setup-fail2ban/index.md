---
date: "2025-06-15T14:09:44+07:00"
draft: false
title: "Setup Fail2Ban"
summary: "Learn how to install, configure, and manage Fail2Ban to secure your SSH server."
categories:
  - Security
tags:
  - ssh
  - fail2ban
  - linux
---

## Introduction

Fail2Ban is a powerful tool to protect your server from brute-force attacks. This guide will walk you through installing and configuring Fail2Ban to secure your SSH service.

## Step 1: Install Fail2Ban

Install Fail2Ban using the following command:

```bash
sudo apt install fail2ban
```

## Step 2: Configure Fail2Ban

Create a local configuration file to override the default settings:

```bash
sudo cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

## Step 3: Configure the SSH Jail

Edit the SSH jail configuration file:

```bash
sudo nano /etc/fail2ban/jail.d/sshd.local
```

Add the following configuration:

```conf
[sshd]
enabled = true
port = ssh
logpath = %(sshd_log)s
backend = systemd
maxretry = 5
bantime = 3600
findtime = 600
```

Configuration rules for other services can be found in [linuxserver/fail2ban-confs](https://github.com/linuxserver/fail2ban-confs/tree/master/filter.d)

## Step 4: Restart Fail2Ban

Apply the changes by restarting the Fail2Ban service:

```bash
sudo systemctl restart fail2ban
```

## Step 5: Verify Fail2Ban Status

Check the overall status of Fail2Ban:

```bash
sudo fail2ban-client status
```

Check the status of the SSH jail:

```bash
sudo fail2ban-client status sshd
```

## Step 6: Unban an IP Address

If you need to unban an IP address, use the following command:

```bash
sudo fail2ban-client set sshd unbanip <IP_ADDRESS>
```

## Conclusion

By following these steps, you have successfully set up Fail2Ban to protect your SSH server from unauthorized access. Regularly monitor the logs and adjust the configuration as needed to enhance your server's security.
