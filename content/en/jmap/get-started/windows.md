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
  jmap:
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
  C:\Program Files\Stalwart JMAP
  C:\Program Files\Stalwart JMAP\bin
  C:\Program Files\Stalwart JMAP\data
  C:\Program Files\Stalwart JMAP\etc
  C:\Program Files\Stalwart JMAP\etc\certs
  C:\Program Files\Stalwart JMAP\etc\private
  ```
- Download the [latest Stalwart JMAP binary](https://github.com/stalwartlabs/jmap-serve/releases/latest/download/stalwart-x86_64-pc-windows-msvc.zip) 
  and uncompress it under ``C:\Program Files\Stalwart JMAP\bin``.
- Download the [latest Stalwart CLI binary](https://github.com/stalwartlabs/jmap-server-cli/releases/latest/download/stalwart-cli-x86_64-pc-windows-msvc.zip)
  and uncompress it under ``C:\Program Files\Stalwart JMAP\bin``.
- Download the [default configuration file](https://raw.githubusercontent.com/stalwartlabs/jmap-server/main/resources/config/config_win.yml) and save it as ``C:\Program Files\Stalwart JMAP\etc\config.yml``. 
- Edit the config.yml file you just downloaded and update the ``encryption-key`` with a random value of at least 64 characters.

*Note*: At the moment only x86 binaries are distributed but an ARM64 Windows version can be compiled from the sources.

## Setting up

Before your Stalwart JMAP server can start accepting connections from JMAP clients, you'll have to configure your hostname and install valid a TLS certificate.

### Hostname

Edit the ``C:\Program Files\Stalwart JMAP\etc\config.yml`` file and update the ``jmap-url`` parameter with the base URL of your JMAP server.
For example, if your JMAP's server hostname is ``jmap.example.org`` then the ``jmap-url`` parameter should look like this:

```yaml
jmap-url: https://jmap.example.org:8080
```

By default Stalwart JMAP listens for connections on the unprivileged ``8080`` port. To use a different port, update the ``jmap-port``
parameter in your ``C:\Program Files\Stalwart JMAP\etc\config.yml`` file. For example, to use the default HTTPS port 443:

```yaml
jmap-port: 443
```

### TLS

Stalwart JMAP requires all HTTP connections to be encrypted over TLS. If you currently don't have a TLS certificate, 
you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/). 
Once you have your certificate ready, copy your certificate and private key to their default locations as follows:

```bash
copy mycert.crt C:\Program Files\Stalwart JMAP\etc\certs\jmap.crt
copy mykey.key C:\Program Files\Stalwart JMAP\etc\private\jmap.key
```

If you already have a proxy such as __nginx__ encrypting incoming HTTP requests, you can disable
TLS encryption on Stalwart JMAP by commenting out the ``jmap-cert-path`` and ``jmap-key-path`` parameters in your
``C:\Program Files\Stalwart JMAP\etc\config.yml`` file. For example:

```yaml
#jmap-cert-path: C:\Program Files\Stalwart JMAP\etc\certs\jmap.crt
#jmap-key-path: C:\Program Files\Stalwart JMAP\etc\private\jmap.key
```

### LMTP

In order to be able to receive emails from the outside world, you need to enable the LMTP service and configure your Mail Transport Agent (MTA)
to deliver messages to Stalwart JMAP over LMTP. Details on how to configure LMTP can be found on the [LMTP section](/jmap/configure/lmtp).
You may also configure LMTP later and continue with the setup instructions.

### SMTP (optional)

The JMAP protocol allows clients to submit e-mail messages for delivery without having to interact with an SMTP server. If you wish to enable
this functionality, you'll have to configure an SMTP relay server. Details on how to configure an SMTP relay server can be found on 
the [SMTP relay section](/jmap/configure/smtp). Additionally, it is recommended that you enable [DKIM](/jmap/manage/domains/#dkim) on all your domain names.

## Start service

To run Stalwart JMAP as a service, follow these instructions:

- Download the [NSSM](http://nssm.cc/download) service manager.
- Run in your terminal:
  ```
  nssm install Stalwart_JMAP
  ```
- Once the NSSM GUI appears, configure the service using the following parameters:
  ```
  Path: C:\Program Files\Stalwart JMAP\bin\stalwart-jmap.exe
  Startup directory: C:\Program Files\Stalwart JMAP
  Arguments: --config=C:\Program Files\Stalwart JMAP\etc\config.yml
  ```
- Click on the Install Service button.

If everything is correct, you should now be able to access the OAuth login screen at ``https://YOUR_HOSTNAME/auth``.

## Update password

For security reasons, it is very important that you change the default administrator password before your JMAP server starts 
accepting connections from the outside world. The default administrator account is ``admin`` with password ``changeme``, which can 
be changed using the following CLI command:

```bash
stalwart-cli.exe -u https://YOUR_HOSTNAME -c changeme account update admin -p NEW_PASSWORD
```

## Next steps

Now that you have Stalwart JMAP up and running, you can proceed to [create](/jmap/manage/accounts) accounts for
your users or [import](/jmap/migrate/overview) them from an existing mail server.

