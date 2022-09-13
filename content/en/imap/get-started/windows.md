---
title: "Windows"
description: ""
lead: ""
date: 2022-01-25T14:41:39+01:00
lastmod: 2022-01-25T14:41:39+01:00
draft: false
images: []
type: docs
menu:
  imap:
    parent: "get-started"
    identifier: "windows"
weight: 102
toc: true
---

## Installing

Unfortunately an automated installer is not yet available for Windows systems. To perform
a manual installation in Windows, follow these steps:

- Create the following directory structure:
  ```
  C:\Program Files\Stalwart IMAP
  C:\Program Files\Stalwart IMAP\bin
  C:\Program Files\Stalwart IMAP\data
  C:\Program Files\Stalwart IMAP\etc
  ```
- Download the [latest Stalwart IMAP binary](https://github.com/stalwartlabs/imap-serve/releases/latest/download/stalwart-x86_64-pc-windows-msvc.zip) 
  and uncompress it under ``C:\Program Files\Stalwart IMAP\bin``.
- Download the [default configuration file](https://raw.githubusercontent.com/stalwartlabs/imap-server/main/resources/config/config_win.yml) and save it as ``C:\Program Files\Stalwart IMAP\etc\config.yml``. 

*Note*: At the moment only x86 binaries are distributed but an ARM64 Windows version can be compiled from the sources.

## Configure JMAP

In order to use Stalwart IMAP, you need a JMAP server backend such as [Stalwart JMAP](https://stalw.art/jmap).
To configure the URL of your JMAP server, edit the ``C:\Program Files\Stalwart IMAP\etc\config.yml`` file and update the ``jmap-url`` parameter with the base URL of your JMAP server.
For example, if your JMAP server's hostname is ``jmap.example.org`` then the ``jmap-url`` parameter should look like this:

```yaml
jmap-url: https://jmap.example.org
```

## Setup TLS

It is highly recommended to enable TLS on your IMAP server. If you currently don't have a TLS certificate, 
you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/). 
Once you have your certificate ready, copy your certificate and private key to their default locations as follows:

```bash
copy mycert.crt C:\Program Files\Stalwart IMAP\etc\imap.crt
copy mykey.key C:\Program Files\Stalwart IMAP\etc\imap.key
```

## Start service

To run Stalwart IMAP as a service, follow these instructions:

- Download the [NSSM](http://nssm.cc/download) service manager.
- Run in your terminal:
  ```
  nssm install Stalwart_IMAP
  ```
- Once the NSSM GUI appears, configure the service using the following parameters:
  ```
  Path: C:\Program Files\Stalwart IMAP\bin\stalwart-imap.exe
  Startup directory: C:\Program Files\Stalwart IMAP
  Arguments: --config=C:\Program Files\Stalwart IMAP\etc\config.yml
  ```
- Click on the Install Service button.

## Next Steps

If everything is correct, you should now be able to connect with an IMAP4 client
on ports 143 or 993 (TLS).
