---
title: "RPC"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "cluster"
    identifier: "rpc"
weight: 504
toc: true
---

## Overview

Remote Procedure Calls (RPC) allow one system to execute a function on a remote system. 
Stalwart JMAP uses RPCs to, among other things, transmit cluster membership status updates and
exchange Raft messages.

The RPC protocol works over TCP, is encrypted with TLS and requires incoming connections to 
be authenticated with a valid encryption key.

## Bind Address

By default the RPC service is bound to all network interfaces (``0.0.0.0``) but this can be configured
by specifying the bind IP address using the ``rpc-bind-addr`` parameter. The default port used by the 
RPC protocol to listen for connections is ``7911`` and it can be altered with the ``rpc-port`` parameter. For example:

```
rpc-bind-addr: 0.0.0.0
rpc-port: 7911
```

Please note that the ``rpc-port`` determines both the TCP port of the RPC service as well as the UDP port of 
the gossip service. For example, a value of ``7911`` will bind the RPC service to ``7911/TCP`` and 
the gossip service to ``7911/UDP``.

## TLS

Transport Layer Security (TLS) is a cryptographic protocol designed to provide communications security over a computer network. 
TLS is considered mandatory and has to be configured in order to enable the RPC service.

The parameters ``rpc-cert-path`` and ``rpc-key-path`` specify the path to the TLS certificate and private key respectively. For example:

```
rpc-cert-path: /usr/local/stalwart-jmap/etc/certs/rpc.crt
rpc-key-path: /usr/local/stalwart-jmap/etc/private/rpc.key
```

If you currently don't have a TLS certificate, you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/).
Alternatively, you may also generate a self-signed certificate as follows

```
openssl req -x509 -nodes -days 1825 -newkey rsa:4096 \
            -subj '/CN=localhost' \ 
            -keyout /usr/local/stalwart-jmap/etc/private/rpc.key \
            -out /usr/local/stalwart-jmap/etc/certs/rpc.crt
```

The ``rpc-tls-domain`` *(soon to be deprecated)* parameter allows to specify the TLS domain name to use when connecting to
a remote RPC service over TLS. When using self-signed certificates, this parameter needs to be absent. Example:

```
rpc-tls-domain: example.org
```

## Connection Parameters

The following options allow the configuration of different connection parameters:

- ``rpc-inactivity-timeout``: Number of milliseconds to wait before closing an inactive RPC connection. Defaults to 300000 milliseconds (5 minutes).
- ``rpc-timeout``: Number of milliseconds to wait before aborting a connection attempt. Defaults to 1000 milliseconds (1 second).
- ``rpc-retries-max``: Maxium number of reconnection attempts to an RPC peer. When this number is exceeded, no further connection attempts to the peer will be made until the node is confirmed alive over the gossip protocol. Defaults to 5 attempts.
- ``rpc-backoff-max``: Maximum backoff in milliseconds, used by the truncated exponential backoff algorithm to calculate the next reconnection attempt. Defaults to 180000 milliseconds (3 minutes).

Example:

```
rpc-inactivity-timeout: 300000 # ms
rpc-timeout: 1000 # ms
rpc-retries-max: 5
rpc-backoff-max: 180000 # ms
```

