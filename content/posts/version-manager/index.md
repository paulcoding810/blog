---
date: "2024-07-25T09:54:36+07:00"
draft: false
summary: A guide to managing versions of popular programming languages and frameworks
title: Version Management for Developers
---

As developers, we often need to work with different versions of programming languages and frameworks. This post covers version management tools for several popular technologies.

## Java

For linux/macOS users, you can use the built-in `java_home` utility:

```sh
/usr/libexec/java_home -V
export JAVA_HOME=`/usr/libexec/java_home -v 1.6.0_65-b14-462`
```

[Reference](https://stackoverflow.com/questions/21964709/how-to-set-or-change-the-default-java-jdk-version-on-macos)

## NodeJS

[nvm (Node Version Manager)](https://github.com/nvm-sh/nvm) is a popular tool for managing Node.js versions:

```sh
nvm use 20.12.2
```

## Python

[uv](https://github.com/astral-sh/uv) is a modern Python package manager and version manager.

```sh
uv venv --python 3.12.0
```

## Flutter

[fvm (Flutter Version Management)](https://github.com/leoafarias/fvm) is an excellent tool for managing Flutter SDK versions.

```sh
fvm use 2.10.0
```

Special thanks to Silas for sharing this awesome tool!

## Rust

Rust uses [rustup](https://rustup.rs/) as its official toolchain manager. It allows you to easily switch between different versions of Rust:

```sh
rustup install 1.68.0
rustup default 1.68.0
```

## Conclusion

That's it