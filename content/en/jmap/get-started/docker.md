---
title: "Docker"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "get-started"
    identifier: "docker"
weight: 103
toc: true
---

## Install

Before you can run the Stalwart JMAP Docker container, you are going to need a TLS certificate. 
If you currently don't have one, you can obtain a free TLS certificate from [Let's Encrypt](https://letsencrypt.org/).
One you have your certificate ready, execute in your terminal:

```bash
docker run -d -ti -p 443:8080 -p 11200:11200 \
           -v <BASE_PATH>:/usr/local/stalwart-jmap \
           --name stalwart-jmap stalwartlabs/jmap-server:latest \
           --lmtp-bind-addr=0.0.0.0 \
           --jmap-url=https://<JMAP_HOSTNAME> \
           --jmap-cert-path=<BASE_PATH>/jmap.crt \
           --jmap-key-path=<BASE_PATH>/jmap.key \
           --db-path=<BASE_PATH>/data \
           --encryption-key=<RANDOM_KEY>
```

Make sure to:
- Replace ``<BASE_PATH>`` with the directory on the Docker host where the Stalwart JMAP data will reside.
- Replace ``<JMAP_HOSTNAME>`` with your server's hostname, for example jmap.example.org.
- Replace ``<RANDOM_KEY>`` with a random string of at least 64 characters.
- Store your TLS certificate under ``<BASE_PATH>/jmap.crt`` and the private key under ``<BASE_PATH>/jmap.key``.

If everything is correct, you should now be able to access the OAuth login screen at ``https://JMAP_HOSTNAME/auth``.

### LMTP

In order to be able to receive emails from the outside world, you need to enable the LMTP service and configure your Mail Transport Agent (MTA)
to deliver messages to Stalwart JMAP over LMTP. Details on how to configure LMTP can be found on the [LMTP section](/jmap/configure/lmtp).
You may also configure LMTP later and continue with the setup instructions.

### SMTP (optional)

The JMAP protocol allows clients to submit e-mail messages for delivery without having to interact with an SMTP server. If you wish to enable
this functionality, you'll have to configure an SMTP relay server. Details on how to configure an SMTP relay server can be found on 
the [SMTP relay section](/jmap/configure/smtp). Additionally, it is recommended that you enable [DKIM](/jmap/manage/domains/#dkim) on all your domain names.


## Install CLI

In order to manage your Stalwart JMAP instance, you are also going to need to install the Stalwart CLI (command line interface).
The CLI tool can be installed on **Linux / MacOS** systems by executing the following command either on the same server where Stalwart JMAP 
is running or any other computer with internet access to your server:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://jmap-client.stalw.art/install.sh | sh
```

On **Windows** download the CLI tool directly from [here](https://github.com/stalwartlabs/jmap-server-cli/releases/latest/download/stalwart-cli-x86_64-pc-windows-msvc.zip).

## Update password

For security reasons, it is very important that you change the default administrator password before your JMAP server starts 
accepting connections from the outside world. The default administrator account is ``admin`` with password ``changeme``, which can 
be changed using the following CLI command:

```bash
stalwart-cli -u https://JMAP_HOSTNAME -c changeme account update admin -p NEW_PASSWORD
```

## Next steps

Now that you have Stalwart JMAP up and running, you can proceed to [create](/jmap/manage/accounts) accounts for
your users or [import](/jmap/migrate/overview) them from an existing mail server.
