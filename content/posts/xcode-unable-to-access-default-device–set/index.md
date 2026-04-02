---
date: "2025-05-31T21:32:00+07:00"
draft: false
title: "Fixing 'Unable to Access Default Device–Set' in Xcode"
summary: "A guide to resolving the 'Unable to Access Default Device–Set' error in Xcode when creating a new simulator."
categories:
  - Code
tags:
  - Xcode
  - Simulator
  - Troubleshooting
---

# Fixing 'Unable to Access Default Device–Set' in Xcode

## TL;DR

If you encounter "Unable to access default device–set" when creating simulators in Xcode, fix directory ownership:

```sh
sudo chown -R $(whoami):staff ~/Library/Developer
```

Restart XCode

Then reimport the simulator runtime and recreate your simulators.

## The Problem

When attempting to create a new simulator in Xcode, you may encounter:

```text
Unable to access default device–set
```

This error occurs due to permission issues with the `~/Library/Developer/CoreSimulator/Devices` directory. Apple Developer Forums discuss this issue ([thread 1](https://developer.apple.com/forums/thread/132281), [thread 2](https://developer.apple.com/forums/thread/664398)), but lack clear solutions.

## Troubleshooting Steps

I attempted several fixes that didn't work:

- Restarting the Mac
- Reinstalling Xcode
- Changing the `xcode-select` path:
  ```sh
  sudo xcode-select -s ~/Downloads/Xcode.app/
  ```
- Manually installing the Simulator runtime ([guide](https://developer.apple.com/documentation/xcode/installing-additional-simulator-runtimes))

## Root Cause

Running these commands failed with permission errors:

```sh
xcrun simctl list devices
xcodebuild -importPlatform ~/Downloads/iOS_18.2_Simulator_Runtime.dmg
```

The error logs revealed:

```text
Error Domain=com.apple.CoreSimulator.SimError Code=400 "Unable to determine SimDeviceSet for subscriptions, set_path=~/Library/Developer/CoreSimulator/Devices"
```

The `~/Library/Developer` directory was owned by `root` instead of your user:

```sh
mkdir ~/Library/Developer/CoreSimulator
# Permission denied
```

## The Solution

Change ownership to match your `~/Library` directory:

```sh
ll ~/Library
# drwx------@ 101 paul  staff  3232  7 Feb 08:01 Library
```

```sh
sudo chown -R paul:staff ~/Library/Developer
```

Then reimport the simulator runtime:

```sh
xcodebuild -importPlatform ~/Downloads/iOS_18.2_Simulator_Runtime.dmg
```

Success:

```text
importing ~/Downloads/iOS_18.2_Simulator_Runtime.dmg
Registered as 9A66D486-5FAE-436A-B0E3-DC2AA9F9****.
```

You can now create simulators via **Xcode → Window → Devices and Simulators → +**.
