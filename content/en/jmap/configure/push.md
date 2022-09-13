---
title: "Push Notifications"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "push"
weight: 208
toc: true
---

## Overview

Push notifications allow JMAP clients to efficiently receive updates almost instantly
and stay in sync with data changes on the JMAP server.
There are two different mechanisms by which a client can receive push notifications
from Stalwart JMAP:

- __Event source__ allows clients to hold transport connections open over HTTP to receive 
  push notifications directly from the server.

- __Push subscriptions__ in which a client registers a push service with the server. Each time there
  is a state change a notification is posted to the push service which in turn is responsible 
  for routing these to the client.

## Push Subscriptions

JMAP clients can activate push subscriptions by registering a push service URL with Stalwart JMAP.
The server will then make an HTTP POST request to this URL for each push notification it wishes to 
send to the JMAP client.

Even though all notifications sent to the push service URL are encrypted over TLS, for enhanced privacy it is recommended
to enable [web push encryption](https://datatracker.ietf.org/doc/html/rfc8291) by providing Elliptic Curve Diffie-Hellman (ECDH) public keys when registering the push service
with Stalwart JMAP. 

The push subscription service accepts the following parameters:

- ``push-max-total``: Total number of push subscriptions that a given user can activate. Defaults to 100.
- ``push-attempts-max:``: Maximun number of push attempts before a notification is discarded. Defaults to 3.
- ``push-attempt-interval``: Number of milliseconds to wait between push attempts. Defaults to 60000 milliseconds (1 minute).
- ``push-retry-interval``: Number of milliseconds to wait between retry attempts. Defaults to 1000 milliseconds (1 second).
- ``push-timeout``: Number of milliseconds before a connection with a push service URL times out. Defaults to 10000 milliseconds (10 seconds).
- ``push-verify-timeout``: Number of milliseconds to wait for the push service to verify a subscription. Defaults to 60000 milliseconds (1 minute).
- ``push-throttle``: Number of milliseconds to wait before sending a new request to the push service. Defaults to 1000 milliseconds (1 second).

Example:

```
push-max-total: 100
push-attempt-interval: 60000 # ms
push-attempts-max: 3
push-retry-interval: 1000 # ms
push-timeout: 10000 # ms
push-verify-timeout: 60000 # ms
push-throttle: 1000 # ms

```

## Event Source

JMAP clients that support holding transport connections open can connect directly to Stalwart JMAP to receive 
push notifications via a ``text/event-stream`` resource, as described in [event source specification](https://html.spec.whatwg.org/multipage/server-sent-events.html#server-sent-events).
This is a long running HTTP request where the server can push data to the client by appending data without ending the response.

The event source endpoint is listed in the JMAP Session resource object (``/.well-known/jmap``) and is available at 
``/jmap/eventsource/?types={types}&closeafter={closeafter}&ping={ping}``.

In order to avoid overwhelming the client with notifications, throttling can be configured 
to group multiple updates and send them to the client in intervals:

- ``event-source-throttle``: Number of milliseconds to wait before sending a new EventSource event to the client. Defaults to 1000 ms (1 second).

Example:

```
event-source-throttle: 1000 # ms
```

## Conformed RFCs

- [RFC 5116 - An Interface and Algorithms for Authenticated Encryption (AEAD_AES_128_GCM)](https://datatracker.ietf.org/doc/html/rfc5116#section-5.1)
- [RFC 8030 - Generic Event Delivery Using HTTP Push](https://datatracker.ietf.org/doc/html/rfc8030)
- [RFC 8188 - Encrypted Content-Encoding for HTTP](https://datatracker.ietf.org/doc/html/rfc8188)
- [RFC 8291 - Message Encryption for Web Push](https://datatracker.ietf.org/doc/html/rfc8291)
