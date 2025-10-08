---
title: "Content Placeholder"
build:
  render: never
  list: never
  publishResources: false
---

## presiquisite

- get os from https://download.lineageos.org/devices/fajita/builds
- get gapps from https://wiki.lineageos.org/gapps/
- get magisk from https://github.com/topjohnwu/Magisk/releases and rename apk to zip
-

## steps

adb -d reboot sideload
adb -d sideload lineage-20.0-20240624-nightly-fajita-signed.zip
adb -d reboot sideload
adb -d sideload open_gapps-arm64-13.0-pico-20240624.zip
adb -d reboot sideload
adb -d sideload Magisk-v26.4.zip
adb -d reboot
