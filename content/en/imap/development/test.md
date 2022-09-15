---
title: "Tests"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  imap:
    parent: "development"
    identifier: "imap-test"
weight: 602
toc: true
---

## Overview

The following subsections cover the different tests available on Stalwart IMAP.
Before running these tests, make sure you have Rust installed by following the instructions
in the [compile section](/imap/development/compile).

## Base tests

The base tests perform protocol compliance tests as well as basic functionality testing on 
different functions across the Stalwart IMAP code base. 
To run the base test suite execute:

```bash
cargo test
```

## IMAP4 tests

The IMAP test suite performs a full server functionaly test including compliance to the IMAP4rev2/rev1
protocols and its extensions. To run these tests a blank Stalwart JMAP installation is required to be running at
``http://127.0.0.1:8080``.

To run the IMAP test suite execute:

```bash
cargo test imap_tests -- --ignored
```

## Third-party tests

Stalwart IMAP's protocol compliance may be also tested with Dovecot's ImapTest:

- Download [ImapTest](https://www.imapwiki.org/ImapTest/Installation).
- Start a blank Stalwart JMAP instance on ``http://127.0.0.1:8080``.
- Create a test account.
- Run the compliance tests as follows:
    ```
    ./imaptest host=<IMAP_HOSTNAME> port=<IMAP_PORT> \
            user=<JMAP_ACCOUNT> pass=<JMAP_ACCOUNT_SECRET> auth=100 \
            test=<PATH_TO_REPO>/src/tests/resources/imap-test/
    ```

Note: The tests distributed with ImapTest were slightly modified to support the
IMAP4rev2 specification.

## Stress tests

Stress testing Stalwart IMAP can be done with Dovecot's ImapTest:

- Download [ImapTest](https://www.imapwiki.org/ImapTest/Installation).
- Start a blank Stalwart JMAP instance on ``http://127.0.0.1:8080``.
- Create at least 3 test accounts, all using the same password. Store the account names in a file, one account per line.
- Run the stress tests as follows:
    ```
    ./imaptest host=<IMAP_HOSTNAME> port=<IMAP_PORT> \
            userfile=<PATH_TO_ACCOUNT_NAMES_FILE> \
            pass=<JMAP_ACCOUNT_SECRET> \
            mbox=<PATH_TO_TEST_MBOX> \
            auth=100
    ```

## Fuzz

To fuzz Stalwart IMAP server with `cargo-fuzz` execute:

```bash
 $ cargo +nightly fuzz run imap_server
```
