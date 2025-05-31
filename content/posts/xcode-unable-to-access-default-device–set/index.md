---
date: "2025-05-31T21:32:00+07:00"
draft: true
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

When attempting to create a new simulator in Xcode, I encountered the following error:

```text
Unable to access default device–set
```

This issue has been discussed in the Apple Developer Forums ([thread 1](https://developer.apple.com/forums/thread/132281), [thread 2](https://developer.apple.com/forums/thread/664398)), but no definitive solution was provided.

## Troubleshooting Attempts

Here are the steps I tried, none of which resolved the issue:

- Restarting the Mac.
- Reinstalling Xcode.
- Changing the `xcode-select` path:
  ```sh
  sudo xcode-select -s ~/Downloads/Xcode.app/
  ```
- Manually installing the Simulator runtime ([guide](https://developer.apple.com/documentation/xcode/installing-additional-simulator-runtimes)).

## Root Cause and Solution

The issue was related to permissions. Running the following commands without `sudo` failed:

```sh
xcrun simctl list devices
xcodebuild -importPlatform ~/Downloads/iOS_18.2_Simulator_Runtime.dmg
```

The error logs indicated a problem with the `~/Library/Developer/CoreSimulator/Devices` directory:

```text
Error Domain=com.apple.CoreSimulator.SimError Code=400 "Unable to determine SimDeviceSet for subscriptions, set_path=~/Library/Developer/CoreSimulator/Devices"
```

I found a similar bug reported on [Stackoverflow](https://stackoverflow.com/a/26632641). He indicated that the issue was due to the missing `~/Library/Developer/CoreSimulator/Devices` directory.

Upon investigation, I found that the `Developer` directory was owned by `root`, causing permission issues:

```sh
mkdir ~/Library/Developer/CoreSimulator

# -> Permission denied
```

To fix this, I changed the ownership of the `Developer` directory as same as `~/Library` directory:

```text
drwx------@ 101 paul  staff  3232  7 Feb 08:01 Library
```

```sh
sudo chown -R paul:staff ~/Library/Developer
```

After this, I successfully imported the simulator runtime:

```sh
xcodebuild -importPlatform ~/Downloads/iOS_18.2_Simulator_Runtime.dmg
```

```text
importing ~/Downloads/iOS_18.2_Simulator_Runtime.dmg
Registered as 9A66D486-5FAE-436A-B0E3-DC2AA9F9****.
```

Now, I can create new simulators in Xcode without any issues. Navigate to **Xcode -> Window -> Devices and Simulators -> +** to add a new simulator.

Cheers!
