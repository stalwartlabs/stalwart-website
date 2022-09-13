---
title: "Compiling"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  imap:
    parent: "development"
    identifier: "imap-compile"
weight: 601
toc: true
---

## Overview

Compiling Stalwart IMAP from the source has the main advantage that binaries are built
with optimizations specific to your hardware which may result in better performance.
Another advantage is that you may enable or disable features to suit your needs.

To compile Stalwart IMAP you need the latest version of [Rust](https://www.rust-lang.org/).

## Install Rust

The fastest way to install rust is with ``rustup``. On Unix systems run the following in your terminal, then follow the onscreen instructions

```
curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
```

If you are running Windows 64-bit, download and run [rustup‑init.exe](https://rustup.rs), then follow the onscreen instructions. 

## Clone Repo

Clone the Stalwart IMAP repository:

```
git clone https://github.com/stalwartlabs/imap-server.git
cd imap-server
```

## Compile

Compile Stalwart IMAP by executing:

```
cargo build --release
```

Once the compilation process is completed, the Stalwart IMAP
binary will be available under ``target/release/stalwart-imap``.

