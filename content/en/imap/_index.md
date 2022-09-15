---
title: "Stalwart IMAP Server Documentation"
description: ""
lead: ""
date: 2022-01-25T14:40:56+01:00
lastmod: 2022-01-25T14:40:56+01:00
draft: false
images: []
type: docs
---

# Stalwart IMAP Server

Stalwart IMAP is an open-source Internet Message Access Protocol server designed to be secure, fast, robust and scalable.
A JSON Meta Application Protocol (JMAP) backend such as [Stalwart JMAP](/jmap/) is required to use Stalwart IMAP (in other words, Stalwart
IMAP is an imap4-to-jmap proxy).

Key features:

- **IMAP4** full compliance and support for multiple extensions:
  - IMAP4rev2 ([RFC 9051](https://datatracker.ietf.org/doc/html/rfc9051))
  - IMAP4rev1 ([RFC 3501](https://datatracker.ietf.org/doc/html/rfc3501)) 
  - Access Control Lists (ACL) ([RFC 4314](https://datatracker.ietf.org/doc/html/rfc4314))
  - Conditional Store and Quick Resynchronization ([RFC 7162](https://datatracker.ietf.org/doc/html/rfc7162))
  - SORT and THREAD ([RFC 5256](https://datatracker.ietf.org/doc/html/rfc5256))
  - Message Preview Generation ([RFC 8970](https://datatracker.ietf.org/doc/html/rfc8970))
  - And [many other extensions](/imap/development/rfc/#imap4-extensions)...
- **JMAP** backend:
  - Proxies IMAP4 requests to JMAP requests.
  - High-availability and fault-tolerance support when using a [Stalwart JMAP](/jmap/) backend.
  - Full compliance with [JMAP Core](https://datatracker.ietf.org/doc/html/rfc8620) and [JMAP Mail](https://datatracker.ietf.org/doc/html/rfc8621).
- **Secure**:
  - OAuth 2.0 [authorization code](https://www.rfc-editor.org/rfc/rfc8628) and [device authorization](https://www.rfc-editor.org/rfc/rfc8628) flows.
  - Rate limiting.
  - Memory safe (thanks to Rust).


## Get Started

Install Stalwart IMAP on your server by following the instructions for your platform:

- [Linux / MacOS](/imap/get-started/linux/)
- [Windows](/imap/get-started/windows/)
- [Docker](/imap/get-started/docker/)

You may also [compile Stalwart IMAP from from the source](/imap/development/compile/).

## Support

If you are having problems running Stalwart IMAP, you found a bug or just have a question,
do not hesitate to reach us on [Github Discussions](https://github.com/stalwartlabs/imap-server/discussions) or [Discord](https://discord.gg/jVAuShSdNZ).
Additionally you may become a sponsor to obtain priority support from Stalwart Labs Ltd.

## Documentation

Table of Contents

- Get Started
  - [Linux / MacOS](/imap/get-started/linux/)
  - [Windows](/imap/get-started/windows/)
  - [Docker](/imap/get-started/docker/)
- Configuration
  - [Overview](/imap/configure/overview/)
  - [IMAP Server](/imap/configure/imap/)
  - [JMAP Proxy](/imap/configure/proxy/)
  - [Cache](/imap/configure/cache/)
- Development
  - [Compiling](/imap/development/compile/)
  - [Tests](/imap/development/test/)
  - [RFCs conformed](/imap/development/rfc/)
