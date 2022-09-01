---
title: "Web Server"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "general"
weight: 200
toc: true
---

## Overview

Stalwart JMAP includes a web server to handle JMAP requests from clients, which are done using JSON over HTTP. 
This section covers the web server configuration.

## Bind address

The bind address is where Stalwart JMAP will listen for incoming HTTP requests. It is configured using the
``jmap-bind-addr`` and ``jmap-port`` parameters:

- ``jmap-bind-addr``: IP address to bind the HTTP service to, or specify ``0.0.0.0`` to bind to all network interfaces. Defaults to ``0.0.0.0``.
- ``jmap-port``: Port to listen for HTTP connections, defaults to ``8080``.

Example:

```
jmap-bind-addr: 0.0.0.0
jmap-port: 8080
```

## JMAP URL

The JMAP URL parameter ``jmap-url`` sets the base URL that will be advertised to JMAP clients when requesting a JMAP Session object.
It is important that this value is correct, otherwise clients won't be able to make JMAP requests.

Example:

```
jmap-url: https://jmap.example.org
```

## TLS

Transport Layer Security (TLS) is a cryptographic protocol designed to provide communications security over a computer network. 
TLS has to be enabled in Stalwart JMAP to keep your users' information secure. The only exception is that you run Stalwart JMAP behind a
proxy that encrypts all incoming HTTP connections.

The parameters ``jmap-cert-path`` and ``jmap-key-path`` specify the path to the TLS certificate and private key respectively. For example:

```
jmap-cert-path: /etc/stalwart-jmap/certs/jmap.crt
jmap-key-path: /etc/stalwart-jmap/private/jmap.key
```

If you currently don't have a TLS certificate, you can obtain one for free from [Let's Encrypt](https://letsencrypt.org/).

## Logging

The logging level is configured with the ``log-level`` parameter which accepts the following values:

- ``error``: Only log very serious errors.
- ``warn``: Log hazardous situations in addition to serious errors.
- ``info``: Log useful information plus all kind of errors.
- ``debug``: Log lower priority information useful to debug problem.
- ``trace``: Log everything including very low priority, often extremely verbose, information.

The default logging level is ``info``. Example:

```
log-level: info
```

## CORS Policy

In order to allow maximum interoperability with web clients running on other URLs, Stalwart JMAP uses
a permissive CORS policy. To enforce a strict CORS policy set the ``strict-cors`` parameter to ``true``.

Example:

```
strict-cors: false
```

## Cache

Caching is employed to increase performance by keeping some commonly accessed information in memory, which reduces the 
number of database operations required to serve a request.
Use the following parameters to fine tune the cache eviction policy:

- ``cache-size-ids``: Maximum size in bytes of the ID cache, defaults to 33554432 bytes (32 MB).
- ``cache-tti-ids``: Time-to-idle in seconds of the ID cache, defaults to 3600 seconds (1 hour).
- ``cache-tti-sharings``: Time-to-idle in seconds of the Shared Documents cache, defaults to 300 seconds (5 minutes).
- ``cache-tti-acl``: Time-to-idle in seconds of the ACL cache, defaults to 3600 seconds (1 hour).
- ``cache-tti-recipients``: Time-to-idle in seconds of the LMTP recipients cache, defaults to 86400 seconds (1 day).

Example:

```
cache-size-ids: 33554432
cache-tti-ids: 3600
cache-tti-sharings: 300
cache-tti-acl: 3600
cache-tti-recipients: 86400
```

## Worker Pool

On startup Stalwart JMAP creates a thread pool where blocking tasks are executed. By default the number of threads
started equals the number of CPUs in the system. The size of the thread pool can be changed using the ``worker-pool-size``,
for example:

```
worker-pool-size: 16
```

