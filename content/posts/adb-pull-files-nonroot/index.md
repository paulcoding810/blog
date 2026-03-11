---
date: "2026-03-10T21:33:26+07:00"
title: "Pulling Files from /data/data Without Root"
summary: "Use `adb run-as` and tar tricks to export app data without rooting the device."
categories:
  - Android
  - Code
tags:
  - adb
  - run-as
  - debugging
  - ssh
---

{{< gpt >}}

Android Studio's Device File Explorer accesses `/data/data` via `run-as` for debuggable apps or root, unlike standard `adb shell` commands.

## Why `/data/data` Access Differs

- **For debuggable apps**: Android Studio uses `run-as` to switch to the app's UID.
- **For other apps**: Requires root or engineering builds.

## Accessing App Data with `run-as`

For a debuggable app (e.g., `com.example.app`), run:

```bash
adb shell run-as com.example.app
cd /data/data/com.example.app
ls
```

Pull a single file:

```bash
adb exec-out run-as com.example.app cat databases/my.db > my.db
```

## Full Access Without Root

Standard `adb shell ls /data/data` fails with "Permission denied" unless rooted:

```bash
adb root  # Only on emulators/engineering builds
adb shell ls /data/data
```

## Summary of Access Methods

| Method                         | Access to `/data/data`   |
| ------------------------------ | ------------------------ |
| `adb shell`                    | ❌ No                    |
| `adb shell run-as com.app`     | ✅ Only that app         |
| `adb root`                     | ✅ Full access           |
| Android Studio Device Explorer | ✅ Uses `run-as` or root |

## Pulling Entire Directories

Export a folder (e.g., `files/`) as tar:

```bash
adb exec-out run-as com.example.app tar -cf - files/ > files.tar
```

Extract locally:

```bash
tar -xf files.tar
```

Export entire app data:

```bash
adb exec-out run-as com.example.app tar -cf - . > appdata.tar
```

## Pushing Files Back

Push a file into app data:

```bash
adb push mydb.db /data/local/tmp/
adb shell run-as com.example.app cp /data/local/tmp/mydb.db databases/
```

## Troubleshooting `run-as`

If it fails with "package not debuggable", ensure your build is debuggable:

```gradle
buildTypes {
    debug {
        debuggable true
    }
}
```

## Interactive Shell as App User

Spawn a shell under the app's UID:

```bash
adb shell run-as com.example.app sh
```

## Command Summary

| Goal             | Command                                                     |
| ---------------- | ----------------------------------------------------------- |
| Pull single file | `adb exec-out run-as com.app cat file > file`               |
| Pull folder      | `adb exec-out run-as com.app tar -cf - folder > folder.tar` |
| Pull entire data | `adb exec-out run-as com.app tar -cf - . > data.tar`        |
| Become app user  | `adb shell run-as com.app sh`                               |
