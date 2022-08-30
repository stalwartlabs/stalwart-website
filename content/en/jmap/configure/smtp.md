---
title: "SMTP Relay"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "smtp"
weight: 200
toc: true
---

## Overview

The JMAP protocol allows clients to submit e-mail messages for delivery without having to
interact with an SMTP server. This is ideal for web-based clients as it allows them to send e-mail 
messages over HTTP without relying on proprietary protocols.

In order to enable the ``EmailSubmission`` functionality, you need an SMTP server configured
to relay messages on behalf of your users. Each time a JMAP client submits a message for delivery, 
Stalwart JMAP will send the message through the configured SMTP relay thus making
the entire e-mail submission process transparent to the user.

## Configuration

Just these two parameters are required to enable e-mail submissions on Stalwart JMAP:

- ``smtp-relay-host``: The IP address of the SMTP relay host.
- ``smtp-relay-port``: The port where the SMTP relay host is listening for connections.

Additionally the following parameters can be configured:

- ``smtp-relay-tls``: If set to ``true``, TLS will be used by default. Defaults to ``true``.
- ``smtp-relay-auth``: Account name to use to authenticate with the SMTP server.
- ``smtp-relay-secret``: Account secret to use to authenticate with the SMTP server.
- ``smtp-relay-timeout``: Timeout in milliseconds, defaults to 60000 milliseconds.

Example using ``foo`` and ``bar`` as authentication credentials:

```
smtp-relay-host: 127.0.0.1
smtp-relay-port: 587
smtp-relay-auth: foo
smtp-relay-secret: bar
smtp-relay-tls: true
smtp-relay-timeout: 60000 # ms
```

## DKIM

E-mail messages sent via Stalwart JMAP can be signed using __DKIM__ (Domain Keys Identified Mail),
which is an email authentication technique that allows the receiver to check that an email was indeed 
sent and authorized by the owner of that domain.
To enable DKIM on your domain, please refer to the [DKIM - Domain management section](/manage/domains#dkim).

