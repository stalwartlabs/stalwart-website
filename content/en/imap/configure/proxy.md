---
title: "JMAP Proxy"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  imap:
    parent: "configure"
    identifier: "proxy"
weight: 300
toc: true
---

## Overview

Stalwart IMAP is in fact a proxy server that converts IMAP requests to [JSON Meta Application Protocol (JMAP)](https://www.rfc-editor.org/rfc/rfc8620.html) requests.
In order to use Stalwart IMAP, you need a JMAP server such as [Stalwart JMAP](https://stalw.art/jmap).

## Configuration

The base URL of the JMAP server is configured with the ``jmap-url`` parameter.

Example:

```yaml
jmap-url: https://localhost:8080
```

## Virtual Folders

Stalwart IMAP includes on each account two virtual folders, an *All Mail* folder
containing all e-mail messages in the JMAP account and a *Shared Folders* folder which
contains all the JMAP folders that were shared to the account.

The default name of these folders is configured with the ``name-shared`` and 
``name-all`` parameters. For example:

```yaml
name-shared: Shared Folders
name-all: All Mail
```

## Trusted Hosts

When accessing JMAP servers running in a distributed environment, requests might be redirected with an
HTTP ``302`` response to the JMAP server that is currently leading the cluster. For security reasons all sensitive 
headers such as those including authorization credentials are removed before redirecting a request. The ``jmap-trusted-hosts``
parameter contains a list of the trusted hostnames where it is safe to include 
any authorization credentials when following a redirect request.

Example:

```yaml
jmap-trusted-hosts: jmap1.example.org;jmap2.example.org
```
