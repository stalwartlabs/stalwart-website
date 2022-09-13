---
title: "IMAP Server"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  imap:
    parent: "configure"
    identifier: "imap-server"
weight: 202
toc: true
---

## Overview

Stalwart IMAP supports both the [IMAP4rev2](https://www.rfc-editor.org/rfc/rfc9051.html) and 
[IMAP4rev1](https://www.rfc-editor.org/rfc/rfc3501) protocols as well as [numerous extensions](/imap/development/rfc/#imap4-extensions).
Clients can connect to Stalwart IMAP over two ports; a TLS encrypted port and a clear-text port which
supports TLS connection upgrades. 
This section covers the basic IMAP server configuration.

## Bind address

The bind address is where Stalwart IMAP will listen for incoming IMAP4 requests. It is configured with the
``bind-addr``, ``bind-port`` and ``bind-port-tls`` parameters:

- ``bind-addr``: IP address to bind the IMAP service to, or specify ``0.0.0.0`` to bind to all network interfaces. Defaults to ``0.0.0.0``.
- ``bind-port``: Port to listen for clear text IMAP connections, defaults to ``143``. Connections can later be upgraded to TLS with the ``STARTTLS`` IMAP command.
- ``bind-port-tls``: Port to listen for TLS IMAP connections, defaults to ``993``.

Example:

```
bind-addr: 0.0.0.0
bind-port: 143
bind-port-tls: 993
```

## TLS

Transport Layer Security (TLS) is a cryptographic protocol designed to provide communications security over a computer network. 
TLS should be enabled in Stalwart IMAP to keep your users' information secure, otherwise clear-text authentication mechanisms
such as ``AUTH=PLAIN`` or ``LOGIN`` will not be offered to clients.

The parameters ``cert-path`` and ``key-path`` specify the path to the TLS certificate and private key respectively. For example:

```
cert-path: /usr/local/stalwart-imap/etc/imap.crt
key-path: /usr/local/stalwart-imap/etc/imap.key
```

If you currently don't have a TLS certificate, you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/).

## Logging

The logging level is configured with the ``log-level`` parameter which accepts the following values:

- ``error``: Only log very serious errors.
- ``warn``: Log hazardous situations in addition to serious errors.
- ``info``: Log useful information plus all kind of errors.
- ``debug``: Log lower priority information useful to debug problem.
- ``trace``: Log everything including very low priority, often extremely verbose, information.

The default logging level is ``info``. Example:

```
log-level: info
```

## Request Size

The maximum size of an IMAP request is controlled with the ``max-request-size`` parameter. This number has to be
large enough to allow users to import messages using the ``APPEND`` command. The default value is 52428800 bytes (50MB).
Example:

```yaml
max-request-size: 52428800
```

## Worker Pool

On startup Stalwart IMAP creates a thread pool where blocking tasks are executed. By default the number of threads
started equals the number of CPUs in the system. The size of the thread pool can be changed using the ``worker-pool-size``,
for example:

```
worker-pool-size: 16
```

