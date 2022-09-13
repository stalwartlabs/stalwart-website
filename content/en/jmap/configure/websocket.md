---
title: "WebSockets"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "websocket"
weight: 209
toc: true
---

## Overview

WebSocket is a protocol described in [RFC6455](https://www.rfc-editor.org/rfc/rfc6455) that allows clients to start two-way
communication channels with a web server. Unlike HTTP that is unidirectional and requires clients
to create a new connection for each request, WebSocket creates a full-duplex persistent connection
with the server in which data can be exchanged in both directions.

JMAP over WebSocket is a subprotocol described in [RFC8887](https://www.rfc-editor.org/rfc/rfc8887.html) that allows the
JMAP protocol to be used over a WebSocket transport layer. 
Using JMAP over WebSocket provides higher performance than JMAP over HTTP by keeping a single presistent connection open between
JMAP clients and servers, which eliminates the performance hit of having to process and authenticate multiple HTTP requests from clients.
When using JMAP over WebSocket, clients are only authenticated once when the connection is started and their credentials remain in effect 
for the duration of the WebSocket connection.

JMAP over WebSocket is enabled by default in Stalwart JMAP and is avaialable to JMAP clients at ``wss://your-domain.org``.

## Configuration

The WebSocket following parameters can be configured:

- ``ws-client-timeout``: Connection timeout for WS clients, the amount of seconds before a WS connection times out. Defaults to 10 seconds.
- ``ws-heartbeat-interval``: Heartbeat interval, the amount of seconds between heartbeat packets sent to WS clients. Defaults to 5 seconds.
- ``ws-throttle``: Notifications throttle, the amout of milliseconds to wait before sending a batch of notifications to a WS client. Defaults to 1000 milliseconds (1 second).

Example:

```
ws-client-timeout: 10 # seconds
ws-heartbeat-interval: 5 # seconds
ws-throttle: 1000 # ms
```

## Conformed RFCs

- [RFC 6455 - The WebSocket Protocol](https://www.rfc-editor.org/rfc/rfc6455)
- [RFC 8307 - Well-Known URIs for the WebSocket Protocol](https://www.rfc-editor.org/rfc/rfc8307)
- [RFC 8441 - Bootstrapping WebSockets with HTTP/2](https://www.rfc-editor.org/rfc/rfc8441)
- [RFC 8887 - A JSON Meta Application Protocol (JMAP) Subprotocol for WebSocket](https://www.rfc-editor.org/rfc/rfc8887)
