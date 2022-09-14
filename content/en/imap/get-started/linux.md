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
  imap:
    parent: "get-started"
    identifier: "linux"
weight: 101
toc: true
---

## Install

Install Stalwart IMAP server by running the following command in your terminal:

```bash
curl --proto '=https' --tlsv1.2 -sSf https://imap.stalw.art/install.sh | sudo sh
```

This command will install Stalwart IMAP under the ``/usr/local/stalwart-imap`` folder
and start the ``stalwart-imap`` service. Please note that root access is required
to perform the installation, if you don't feel comfortable running the install script as root
you may also [download the latest release](https://github.com/stalwartlabs/imap-server/releases) and
perform a manual installation.

## Configure JMAP

In order to use Stalwart IMAP, you need a JMAP server backend such as [Stalwart JMAP](https://stalw.art/jmap).
To configure the URL of your JMAP server, edit the ``/usr/local/stalwart-imap/etc/config.yml`` file and update the ``jmap-url`` parameter with the base URL of your JMAP server.
For example, if your JMAP server's hostname is ``jmap.example.org`` then the ``jmap-url`` parameter should look like this:

```yaml
jmap-url: https://jmap.example.org
```

## Setup TLS

It is highly recommended to enable TLS on your IMAP server. If you currently don't have a TLS certificate, 
you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/). 
Once you have your certificate ready, copy your certificate and private key to their default locations as follows:

```bash
sudo cp mycert.crt /usr/local/stalwart-imap/etc/imap.crt
sudo cp mykey.key /usr/local/stalwart-imap/etc/imap.key
```

## Restart service

Once you have completed the setup instructions, restart your Stalwart IMAP server:

```bash
sudo systemctl restart stalwart-imap
```

Or, if you are using MacOS:

```bash
sudo launchctl kickstart -k stalwart.imap
```

**Important**: On Linux systems, in order to be able to listen on the privileged ports 143 and 993, the Stalwart IMAP
service needs to run as the ``root`` user (this is not the case in MacOS). To run the service as the ``stalwart-imap`` user,
edit the ``/etc/systemd/system/stalwart-imap.service`` file and uncomment the ``User`` and ``Group`` parameters.
Then disable privileged ports with the command ``sudo sysctl -w net.ipv4.ip_unprivileged_port_start=0`` and restart
the service. Alternatively, you may also bind Stalwart JMAP to an unprivileged port such as 1143 and then forward the connections using
iptables such as ``iptables -A PREROUTING -t nat -i eth0 -p tcp --dport 143 -j REDIRECT --to-port 1143``.

## Next Steps

If everything is correct, you should now be able to connect with an IMAP4 client
on ports 143 or 993 (TLS).
