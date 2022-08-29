---
title: "LMTP Delivery"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "lmtp"
weight: 200
toc: true
---

## Overview

In order to be able to receive emails from the outside world, you need to enable the LMTP service and configure your Mail Transport Agent (MTA)
to deliver messages to Stalwart JMAP over LMTP. Local Mail Transfer Protocol (__LMTP__) is a network protocol defined in
[RFC 2033](https://datatracker.ietf.org/doc/html/rfc2033) for transporting mail into systems that do not have a message queue (such as a message store).
The LMTP protocol is practically identical to the SMTP protocol with its service extensions, except a few changes. Despite the similarities, it should not be used
on public networks or as a replacement for SMTP. In fact, LMTP is used in combination with SMTP to deliver messages into user mailboxes.

By default Stalwart JMAP listens for LMTP connections on ``127.0.0.1``, port ``11200`` but it can be changed to use any other port.
It is important though that the LMTP port you choose is not exposed to the Internet as malicious actors could use it to bypass your SPAM filters. For this reason, 
in systems running on a single node, it is recommended to use the loopback address (``127.0.0.1``) as the bind address and, in multi-node setups, to configure 
the `lmtp-trusted-ips` parameter.

## Basic configuration

In order to enable the LMTP service, the following two parameters have to be configured:

- ``lmtp-bind-addr``: IP address to bind the LMTP service to, or use `0.0.0.0` to bind to all network interfaces. Defaults to ``127.0.0.1``.
- ``lmtp-port``: Bind port for the LMTP service. Defaults to ``11200``.

Example: 

```
lmtp-bind-addr: 127.0.0.1
lmtp-port: 11200
```

## TLS configuration

TLS is not necessary when using the loopback address (``127.0.0.1``) as the bind address. In all other cases, it is highly
recommended to enable TLS:

- ``lmtp-cert-path``: Path to the SSL certificate in *PEM* format.
- ``lmtp-key-path``: Path to the private key.
- ``lmtp-tls-only``: When set to `true`, all incoming connections automatically use TLS. If set to `false`, connections are
  started in clear-text and TLS can be enabled by clients using the `STARTTLS` command. 

Example:

```
lmtp-cert-path: /etc/stalwart-jmap/certs/lmtp.crt
lmtp-key-path: /etc/stalwart-jmap/private/lmtp.key
lmtp-tls-only: true
```

## Trusted IPs

In deployments where the SMTP server does not reside in the same machine as Stalwart JMAP, it is necessary to configure
the list of trusted IP addresses from which LMTP connections are allowed.

- ``lmtp-trusted-ips``: List of IP addresses separated by a semicolon.

Example:

``
lmtp-trusted-ips: 192.168.0.1;192.168.0.2
``

## MTA configuration

A Mail Transport Agent with LMTP support is required in order to receive messages from the Internet. Some MTAs that are known
to have LMTP capabilities are [Postfix](https://www.postfix.org/), [Exim](https://www.exim.org/), [Sendmail](https://www.sendmail.org/) 
and [Qmail](https://cr.yp.to/qmail.html) (through an extension).

### Postfix

To enable LMTP delivery on Postfix:

- Install a fresh copy of Postfix, or make sure to remove any existing virtual transports from the configuration file.
- Edit ``/etc/postfix/main.cf`` and add the following lines:

  ```
  virtual_mailbox_domains = example.org
  virtual_transport = lmtp:[127.0.0.1]:11200
  smtpd_recipient_restrictions = reject_unverified_recipient
  ```

- Then, replace:
  - ``example.org`` with your domain name, or you may also add multiple domain names separated by commas.
  - ``127.0.0.1`` and ``11200`` with the LMTP service bind address and port.
- Finally, execute ``sudo postfix reload`` and you should now be able to receive emails from the outside world.

### Exim

Exim uses Unix sockets by default to deliver messages over LMTP. In order to use LMTP over TCP, add
the following router and transport to your configuration file, replacing the default values with 
your setup's bind IP address and port:

```
local_user:
  transport = stalwart_lmtp
  domains = +local_domains
  driver = manualroute
  route_list = "* 127.0.0.1 byname"
  self = send

stalwart_lmtp:
  driver = smtp
  protocol = lmtp
  port = 11200
  rcpt_include_affixes
```

And, to enforce recipient verification over LMTP, the following ACL rule might be employed:

```
deny
  !verify = recipient/callout=no_cache  
  domains = +local_domains
  message = invalid recipient
```

### Sendmail

Sendmail does support LMTP but unfortunately does not support LMTP over TCP. However, it is possible
to use [this Python script](https://fossies.org/linux/mailman/contrib/qmail-lmtp) to add that missing functionality to Sendmail.
To enable LMTP in Sendmail, edit your /etc/sendmail.cf file and add:

```
FEATURE(`local_lmtp´)
FEATURE(`local_lmtp´, `/usr/sbin/lmtp.py 127.0.0.1:11200´)
```

Replace ``127.0.0.1:11200`` with your LMTP service's IP address and port. Finally, download [lmtp.py](https://fossies.org/linux/mailman/contrib/qmail-lmtp)
and save it as ``/usr/sbin/lmtp.py``.

### Qmail

Qmail does not offer LMTP support out of the box. However, you can use the [qmail-lmtp](https://fossies.org/linux/mailman/contrib/qmail-lmtp)
extension to add LMTP support to your qmail installation.

## Clustering

When running Stalwart JMAP in a high availability [cluster](/jmap/cluster/), LMTP connections are acepted by all
peers in the cluster and automatically forwarded to the leader over RPC. No additional configurations are required.

## Conformed RFCs

Extensions:

- [RFC 1870 - SMTP Service Extension for Message Size Declaration](https://www.rfc-editor.org/rfc/rfc1870)
- [RFC 2034 - SMTP Service Extension for Returning Enhanced Error Codes](https://www.rfc-editor.org/rfc/rfc2034)
- [RFC 2920 - SMTP Service Extension for Command Pipelining](https://www.rfc-editor.org/rfc/rfc2920)
- [RFC 3030 - SMTP Service Extensions for Transmission of Large and Binary MIME Messages](https://www.rfc-editor.org/rfc/rfc3030)
- [RFC 3207 - SMTP Service Extension for Secure SMTP over Transport Layer Security](https://www.rfc-editor.org/rfc/rfc3207)
- [RFC 6152 - SMTP Service Extension for 8-bit MIME Transport](https://www.rfc-editor.org/rfc/rfc6152)
- [RFC 6532 - SMTP Extension for Internationalized Email](https://www.rfc-editor.org/rfc/rfc6531)

LMTP/SMTP RFCs:

- [RFC 821 - Simple Mail Transfer Protocol](https://www.rfc-editor.org/rfc/rfc821)
- [RFC 2033 - Local Mail Transfer Protocol](https://www.rfc-editor.org/rfc/rfc2033)
- [RFC 2821 - Simple Mail Transfer Protocol (updates RFC821)](https://www.rfc-editor.org/rfc/rfc2821)
- [RFC 5321 - Simple Mail Transfer Protocol (updates RFC2821)](https://www.rfc-editor.org/rfc/rfc5321)

