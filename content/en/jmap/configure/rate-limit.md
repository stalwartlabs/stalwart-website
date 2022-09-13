---
title: "Rate Limiter"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "rate-limit"
weight: 210
toc: true
---

## Overview

Rate limiting is a strategy to limit network traffic. As the name suggests, it puts a limit on how often 
someone can repeat an action (such as trying to log into an account) within a given time period.
Rate limiting can help mitigate certain types of malicious activity such as brute force attacks. It is also 
useful to reduce the load on your JMAP server.

In Stalwart JMAP, rate limiting works by tracking the IP addresses from which requests are coming from and keeping
a record of how much time passes between each request. Then, Stalwart JMAP measures the number of requests received
from each IP address as well as the time elapsed between each request. If there are too many requests from a 
given IP within a configured timeframe, Stalwart JMAP will reject all further requests coming from that IP address 
for some time until the limit is restored.

## Proxy Setup

When running Stalwart JMAP behind a proxy such as Cloudflare or Amazon CloudFront, the rate limiter needs to
be instructed to obtain the client's IP address from the ``Forwarded`` or ``X-Forwarded-For`` HTTP header rather
than from the socket source address (which most likely is the proxy's address).

This can be done by setting the ``use-forwarded-header`` parameter to ``true``, for example:

```
use-forwarded-header: true
```

When not using a proxy server, make sure that this parameter is set to ``false`` to avoid malicious clients
from forging their source IP address.

## Authentication Limits

The setting ``rate-limit-auth`` controls the amount of authentication requests that can be made
in a timeframe by a given IP address. The format of this parameter is ``<number_of_requests>/<time_in_seconds>``
and the default value is ``10/60`` (10 requests per minute).

Example:

```
rate-limit-auth: 10/60 # num. requests / time
```

## Anonymous Limits

The setting ``rate-limit-anonymous`` controls the amount of requests that an anonymous IP address can make
in a timeframe. The format of this parameter is ``<number_of_requests>/<time_in_seconds>``
and the default value is ``100/60`` (100 requests per minute).

Example:

```
rate-limit-anonymous: 100/60 # num. requests / time
```

## Authenticated Limits

The setting ``rate-limit-authenticated`` controls the amount of requests that an authenticated user can make
in a timeframe. The format of this parameter is ``<number_of_requests>/<time_in_seconds>``
and the default value is ``1000/60`` (1000 requests per minute).

Example:

```
rate-limit-authenticated: 1000/60 # num. requests / time
```

## Concurrent Requests

The parameter ``max-concurrent-requests`` controls the number of requests that an
authenticated user can make concurrently to Stalwart JMAP. The default setting is 4 concurrent requests.

Example:

```
max-concurrent-requests: 4
```

## Concurrent Uploads

The parameter ``max-concurrent-uploads`` controls the number of uploads processes that an
authenticated user can start concurrently. The default setting is 4 concurrent uploads.

Example:

```
max-concurrent-uploads: 4
```

