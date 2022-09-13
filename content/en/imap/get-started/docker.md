---
title: "Docker"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  imap:
    parent: "get-started"
    identifier: "docker"
weight: 103
toc: true
---

## Install

Before you can run the Stalwart IMAP Docker container, you are going to need a TLS certificate. 
If you currently don't have one, you can obtain a free TLS certificate from [Let's Encrypt](https://letsencrypt.org/).
One you have your certificate ready, execute in your terminal:

```bash
docker run -d -ti -p 143:1443 -p 993:1993 \
           -v <BASE_PATH>:/usr/local/stalwart-imap \
           --name stalwart-imap stalwartlabs/imap-server:latest \
           --bind-port=1443 \
           --bind-port-tls=1993 \
           --jmap-url=https://<JMAP_HOSTNAME> \
           --cert-path=<BASE_PATH>/imap.crt \
           --key-path=<BASE_PATH>/imap.key \
           --cache-dir=<BASE_PATH>/data
```

Make sure to:
- Replace ``<BASE_PATH>`` with the directory on the Docker host where the Stalwart IMAP data will reside.
- Replace ``<JMAP_HOSTNAME>`` with your JMAP server's hostname, for example jmap.example.org.
- Store your TLS certificate under ``<BASE_PATH>/imap.crt`` and the private key under ``<BASE_PATH>/imap.key``. 

If everything is correct, you should now be able to connect with an IMAP4 client
on ports 143 or 993 (TLS).

