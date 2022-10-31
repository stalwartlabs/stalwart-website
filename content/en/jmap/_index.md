---
title: "Stalwart JMAP Server Documentation"
description: ""
lead: ""
date: 2022-01-25T14:40:56+01:00
lastmod: 2022-01-25T14:40:56+01:00
draft: false
images: []
type: docs
---

# Stalwart JMAP Server

Stalwart JMAP is an open-source JSON Meta Application Protocol server designed to be secure, fast, robust and scalable.
JMAP is a modern protocol for synchronising data such as mail, calendars, or contacts that makes much more efficient use of network resources.

Key features:

- **JMAP** full compliance:
  - JMAP Core ([RFC 8620](https://datatracker.ietf.org/doc/html/rfc8620))
  - JMAP Mail ([RFC 8621](https://datatracker.ietf.org/doc/html/rfc8621))
  - JMAP over WebSocket ([RFC 8887](https://datatracker.ietf.org/doc/html/rfc8887))
  - JMAP for Sieve Scripts ([DRAFT-SIEVE-12](https://www.ietf.org/archive/id/draft-ietf-jmap-sieve-12.html)).
- **IMAP4** full compliance:
  - IMAP4rev2 ([RFC 9051](https://datatracker.ietf.org/doc/html/rfc9051))
  - IMAP4rev1 ([RFC 3501](https://datatracker.ietf.org/doc/html/rfc3501)) 
  - Numerous [extensions](https://stalw.art/imap/development/rfc/#imap4-extensions) supported.
- **Flexible and robust** message storage:
  - Sieve Script message filtering with support for [all extensions](/jmap/configure/sieve/#conformed-rfcs).
  - Full-text search support available in 17 languages.
  - Local Mail Transfer Protocol ([LMTP](https://datatracker.ietf.org/doc/html/rfc2033)) message ingestion.
  - [RocksDB](http://rocksdb.org/) backend.
- **Secure**:
  - OAuth 2.0 [authorization code](https://www.rfc-editor.org/rfc/rfc8628) and [device authorization](https://www.rfc-editor.org/rfc/rfc8628) flows.
  - Domain Keys Identified Mail ([DKIM](https://www.rfc-editor.org/rfc/rfc6376)) message signing.
  - Access Control Lists (ACLs).
  - Rate limiting.
  - Memory safe (thanks to Rust).
- **Scalable and fault-tolerant**:
  - Node autodiscovery and failure detection over gossip protocol.
  - Replication and cluster consensus over the [Raft](https://raft.github.io/) protocol.
  - Read-only replicas.
  - No third-party replication or cluster coordination software required.

## Get Started

Install Stalwart JMAP on your server by following the instructions for your platform:

- [Linux / MacOS](/jmap/get-started/linux/)
- [Windows](/jmap/get-started/windows/)
- [Docker](/jmap/get-started/docker/)

You may also [compile Stalwart JMAP from the source](/jmap/development/compile/).

## Support

If you are having problems running Stalwart JMAP, you found a bug or just have a question,
do not hesitate to reach us on [Github Discussions](https://github.com/stalwartlabs/jmap-server/discussions) or [Discord](https://discord.gg/jtgtCNj66U).
Additionally you may become a sponsor to obtain priority support from Stalwart Labs Ltd.

## Documentation

Table of Contents

- Get Started
  - [Linux / MacOS](/jmap/get-started/linux/)
  - [Windows](/jmap/get-started/windows/)
  - [Docker](/jmap/get-started/docker/)
- Configuration
  - [Overview](/jmap/configure/overview/)
  - [Web Server](/jmap/configure/webserver/)
  - [Database](/jmap/configure/database/)
  - [LMTP Delivery](/jmap/configure/lmtp/)
  - [SMTP Relay](/jmap/configure/smtp/)
  - [OAuth Settings](/jmap/configure/oauth/)
  - [JMAP Settings](/jmap/configure/jmap/)
  - [Push Notifications](/jmap/configure/push/)
  - [WebSockets](/jmap/configure/websocket/)
  - [Sieve Filters](/jmap/configure/sieve/)
  - [Rate Limiter](/jmap/configure/rate-limit/)
- Management
  - [Overview](/jmap/manage/overview/)
  - [Accounts](/jmap/manage/accounts/)
  - [Domains](/jmap/manage/domains/)
  - [Groups](/jmap/manage/groups/)
  - [Mailing Lists](/jmap/manage/lists/)
  - [Access Control Lists](/jmap/manage/acl/)
- Migration
  - [Overview](/jmap/migrate/overview/)
  - [Import Accounts](/jmap/migrate/accounts/)
  - [Import Mailboxes](/jmap/migrate/mailboxes/)
- Clustering
  - [Quick Start](/jmap/cluster/quick-start/)
  - [Gossip Protocol](/jmap/cluster/gossip/)
  - [Consensus Protocol](/jmap/cluster/raft/)
  - [RPC](/jmap/cluster/rpc/)
- Development
  - [Compiling](/jmap/development/compile/)
  - [Tests](/jmap/development/test/)
  - [RFCs conformed](/jmap/development/rfc/)
