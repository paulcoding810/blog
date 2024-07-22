+++
title = 'Common ADB Commnands'
summary = 'List of my most used ADB commnad'
tags = ['adb', 'android'] 
categories = ["Code"]
date = 2024-07-22T09:22:25+07:00
draft = false
+++

## Wireless adb

### Using wifi

```sh
adb connect 192.168.1.5:312343
```

### Using pair code

Pair by ip, then input the code

```sh
adb pair 192.168.1.5:123123
```

### Using TCP

```sh
adb tcpip 5555
adb connect 192.168.1.15

```

## Config Screen

```sh
adb shell wm size 1080x1920
adb shell wm density 390
```

## Proxy

```sh
# get proxy
adb shell settings get global http_proxy
# set proxy
adb shell settings put global http_proxy server_ip:server_port 
# remove proxy
adb shell settings put global http_proxy :0
```

## Reverse TCP

```sh
adb reverse tcp:8081 tcp:8081
```

## Files

```sh
# push file
adb push avatar.jpg /storage/emulated/0/Download
# pull file
adb shell pm path com.kugou.shiqutouch
adb pull "/data/app/~~xpKM66cgiEbqpkAmw5bUnQ==/com.kugou.shiqutouch-UG94g9pPeK1JHJrxI7yGfQ==/base.apk" ~/Downloads/app.apk
 ```
