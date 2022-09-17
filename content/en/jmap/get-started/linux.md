---
title: "Linux / MacOS"
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
    identifier: "linux"
weight: 101
toc: true
---

## Install

Install Stalwart JMAP server by running the following command in your terminal:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://jmap.stalw.art/install.sh | sudo sh
```

Once the installation is completed, Stalwart JMAP will be available under the ``/usr/local/stalwart-jmap``
directory. The installation script will also create the ``stalwart-jmap`` account and start the ``stalwart-jmap`` systemd/launchd service.

Please note that _root access_ is required to perform the installation, if you don't feel comfortable running the install script as root
you may also [download the latest release](https://github.com/stalwartlabs/jmap-server/releases) and
perform a manual installation.

## Set up

You should now be able to access your Stalwart JMAP web server at ``https://localhost:8080`` (using a self-signed certificate), but
before it can start accepting connections from JMAP clients, you'll have to configure your hostname and install valid a TLS certificate.

### Hostname

Edit the ``/usr/local/stalwart-jmap/etc/config.yml`` file and update the ``jmap-url`` parameter with the base URL of your JMAP server.
For example, if your JMAP's server hostname is ``jmap.example.org`` then the ``jmap-url`` parameter should look like this:

```yaml
jmap-url: https://jmap.example.org:8080
```

By default Stalwart JMAP listens for connections on the unprivileged ``8080`` port. To use a different port, update the ``jmap-port``
parameter in your ``/usr/local/stalwart-jmap/etc/config.yml`` file. For example, to use the default HTTPS port 443:

```yaml
jmap-port: 443
```

Please note that you'll have to run Stalwart JMAP as _root_ if you choose a privileged port such as 443.

### TLS

Stalwart JMAP requires all HTTP connections to be encrypted over TLS. If you currently don't have a TLS certificate, 
you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/). 
Once you have your certificate ready, copy your certificate and private key to their default locations as follows:

```bash
sudo cp mycert.crt /usr/local/stalwart-jmap/etc/certs/jmap.crt
sudo cp mykey.key /usr/local/stalwart-jmap/etc/private/jmap.key
```

If you already have a proxy such as __nginx__ encrypting incoming HTTP requests, you can disable
TLS encryption on Stalwart JMAP by commenting out the ``jmap-cert-path`` and ``jmap-key-path`` parameters in your
``/usr/local/stalwart-jmap/etc/config.yml`` file. For example:

```yaml
#jmap-cert-path: /usr/local/stalwart-jmap/etc/certs/jmap.crt
#jmap-key-path: /usr/local/stalwart-jmap/etc/private/jmap.key
```

### LMTP

In order to be able to receive emails from the outside world, you need to enable the LMTP service and configure your Mail Transport Agent (MTA)
to deliver messages to Stalwart JMAP over LMTP. Details on how to configure LMTP can be found on the [LMTP section](/jmap/configure/lmtp).
You may also configure LMTP later and continue with the setup instructions.

### SMTP (optional)

The JMAP protocol allows clients to submit e-mail messages for delivery without having to interact with an SMTP server. If you wish to enable
this functionality, you'll have to configure an SMTP relay server. Details on how to configure an SMTP relay server can be found on 
the [SMTP relay section](/jmap/configure/smtp). Additionally, it is recommended that you enable [DKIM](/jmap/manage/domains/#dkim) on all your domain names.

## Restart service

Once you have completed the setup instructions, restart your Stalwart JMAP server:

```bash
sudo systemctl restart stalwart-jmap
```

Or, if you are using MacOS:

```bash
sudo launchctl kickstart -k stalwart.jmap
```

If everything is correct, you should now be able to access the OAuth login screen at ``https://YOUR_HOSTNAME/auth``.

## Update password

For security reasons, it is very important that you change the default administrator password before your JMAP server starts 
accepting connections from the outside world. The default administrator account is ``admin`` with password ``changeme``, which can 
be changed using the following Stalwart Command Line Interface (CLI) tool command:

```bash
/usr/local/stalwart-jmap/bin/stalwart-cli -u https://YOUR_HOSTNAME -c changeme account update admin -p NEW_PASSWORD
```

## Next steps

Now that you have Stalwart JMAP up and running, you can proceed to [create](/jmap/manage/accounts) accounts for
your users or [import](/jmap/migrate/overview) them from an existing mail server.
