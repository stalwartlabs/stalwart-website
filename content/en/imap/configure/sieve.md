---
title: "ManageSieve"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "rate-limit"
weight: 204
toc: true
---

## Overview

Sieve ([RFC5228](https://www.rfc-editor.org/rfc/rfc5228.html)) is a scripting language for filtering email messages at or around the time of final delivery.
It is suitable for running on a mail server where users may not be allowed to execute arbitrary programs 
as it has no user-controlled loops or the ability to run external programs.
Sieve is a data-driven programming language, similar to earlier email filtering languages such as procmail and 
maildrop, and earlier line-oriented languages such as sed and AWK: it specifies conditions to match and actions 
to take on matching.

Stalwart IMAP includes support for [ManageSieve](https://datatracker.ietf.org/doc/html/rfc5804) which allows
users to upload and manage their Sieve scripts. Requests received by the ManageSieve server are sent to the JMAP
server using the [JMAP for Sieve Scripts](https://www.ietf.org/archive/id/draft-ietf-jmap-sieve-12.html) protocol.

### Enabling ManageSieve

ManageSieve is enabled by specifying the port number where the server will listen for incoming connections, which is configured
with the ``bind-port-sieve`` parameter. For example, to use the default ManageSieve port ``4190``:

```
bind-port-sieve: 4190
```

### Configuring ManageSieve

Since the ManageSieve server is actually a ManageSieve-to-JMAP-for-Sieve proxy, all configuration of the Sieve compiler and runtime
has to be done at the JMAP server. For Stalwart JMAP please refer to the [Sieve configuration section](/jmap/configure/sieve/).

