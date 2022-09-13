---
title: "Compiling"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "development"
    identifier: "compile"
weight: 601
toc: true
---

## Overview

Compiling Stalwart JMAP from the source has the main advantage that binaries are built
with optimizations specific to your hardware which may result in better performance.
Another advantage is that you may enable or disable features to suit your needs.

To compile Stalwart JMAP you need the latest version of [Rust](https://www.rust-lang.org/) as well as Clang version 10
or greater.

## Install Rust

The fastest way to install rust is with ``rustup``. On Unix systems run the following in your terminal, then follow the onscreen instructions

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

If you are running Windows 64-bit, download and run [rustup‑init.exe](https://rustup.rs), then follow the onscreen instructions. 

## Install Clang

If you are using Ubuntu Linux, you may install clang running:

```
sudo apt install clang
```

On MacOs execute:

```
brew install --with-toolchain llvm
```

## Clone Repo

Clone the Stalwart JMAP repository:

```
git clone https://github.com/stalwartlabs/jmap-server.git
cd jmap-server
```

## Compile

Compile Stalwart JMAP by executing:

```
cargo build --release
```

Once the compilation process is completed, the Stalwart JMAP
binary will be available under ``target/release/stalwart-jmap``.

