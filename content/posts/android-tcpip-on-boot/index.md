---
date: '2024-11-28T12:19:55+07:00'
draft: false
title: 'How to Enable ADB Over TCP/IP on Android Boot'
summary: 'Configure Android to automatically set ADB TCP port 5555 on system startup'
categories:
  - Android
  - Development
tags:
  - adb
  - android
  - root
  - magisk
---

## Prerequisites

- Android device rooted with Magisk
- Basic knowledge of ADB and shell commands

For more information about Magisk boot scripts, see the [official documentation](https://github.com/topjohnwu/Magisk/blob/master/docs/guides.md#boot-scripts).

## Setup Instructions

1. Create a new script file named `script.sh` in the `/data/adb/service.d` directory with the following contents:

```sh
#!/system/bin/sh
setprop service.adb.tcp.port 5555
stop adbd
start adbd
```

2. Make the script executable by running:

```sh
chmod +x /data/adb/service.d/script.sh
```

3. Reboot your device for the changes to take effect.

After rebooting, your device will automatically enable ADB over TCP/IP on port 5555. You can connect to it using:

```sh
adb connect <device-ip>
```

## Notes

- Make sure your device is connected to the same network as your computer
- For security reasons, only enable this on trusted networks
- The ADB TCP connection will be available as long as the device is powered on and connected to the network
