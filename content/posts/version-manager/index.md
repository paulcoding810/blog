---
date: "2024-07-25T09:54:36+07:00"
draft: true
summary: How to control your language version on Macos
title: Version Manager
---

### Java

[reference](https://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-macos)

```sh
/usr/libexec/java_home -V
export JAVA_HOME=`/usr/libexec/java_home -v 1.6.0_65-b14-462`
```

### NodeJS

[nvm](https://github.com/nvm-sh/nvm)

```sh
nvm use 20.12.2
```
