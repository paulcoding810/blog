---
date: '2025-01-20T22:57:26+07:00'
draft: false
title: 'Create an Android Binary'
summary: 'How to create and deploy Android binaries using NDK cross-compilation'
categories:
- Code
tags:
- android
- clang
- ndk
---

## Introduction

Creating Android binaries from C programs requires cross-compilation, as Android devices typically use different architectures (ARM, x86) compared to development machines. This guide will walk you through the process of creating and deploying Android binaries using the Android NDK.

## Prerequisites

- Android NDK (Native Development Kit) installed
- Android SDK with ADB (Android Debug Bridge)
- A C program to compile
- An Android device or emulator for testing

## Cross-Compilation Process

A simple C program sample:

```c
#include <stdio.h>

int main() {
    printf("Hello World");
    return 0;
}
```

The basic workflow involves using the NDK's Clang compiler to target specific Android architectures. Here's the command structure:

```bash
~/Library/Android/sdk/ndk/<version>/toolchains/llvm/prebuilt/darwin-x86_64/bin/clang \
    -target armv7a-linux-androideabi21 \
    -o hello-android \
    hello.c
```

## Deploying to Device

After compilation, use these commands to deploy and run your binary:

```bash
# Push binary to device
adb push hello-android /data/bin/hello

# Set executable permissions
adb shell chmod 777 /data/bin/hello

# Run the binary
adb shell /data/bin/hello
```

## Notes

- The target `-target armv7a-linux-androideabi21` specifies ARMv7 architecture with Android API level 21
- Adjust the target based on your device's architecture (ARM, ARM64, x86, etc.)
- Ensure your device has proper permissions to execute binaries in the `/data/bin` directory
