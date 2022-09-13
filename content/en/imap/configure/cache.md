---
title: "Cache"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  imap:
    parent: "configure"
    identifier: "cache"
weight: 204
toc: true
---

## Overview

Since JMAP uses *strings* as unique identifiers and IMAP uses *integers* to uniquely identify messages and MODSEQs, it is necessary to
keep in the file system a table that maps JMAP identifiers to IMAP identifiers and vice versa. This table is a key-value store referred to as the *cache* and
stores information such as:

- ``UID`` (IMAP) to ``id`` (JMAP) mappings and vice versa.
- ``UIDVALIDITY`` identifiers for each IMAP folder.
- ``UIDNEXT`` for each IMAP folder.
- ``MODSEQ`` (IMAP) to ``state`` (JMAP) mappings and vice versa (used by the [CONDSTORE](https://www.rfc-editor.org/rfc/rfc4551) extension).
- ``HIGHESTMODSEQ`` for each IMAP folder (used by the [CONDSTORE](https://www.rfc-editor.org/rfc/rfc4551) extension).

Even though the contents of the cache are not critical, it __should not__ be deleted as it will force
IMAP clients to resynchronize all folders and messages with the server.

## Configuration

The path to where cache resides in the file system is configured with the ``cache-dir`` parameter and its default
value is ``/usr/local/stalwart-imap/data``.

Example:

```
db-path: /usr/local/stalwart-imap/data
```

## Maintenance

Stalwart IMAP includes a job scheduler service in charge of running cache maintenance tasks such as purging expired or deleted entries. 
The time when the cache purge task runs is configured with the ``cache-purge-every`` parameter using a 
simplified [crontab](https://en.wikipedia.org/wiki/Cron) expression:

``
<minute> <hour> <week-day>
``

Where:

- ``<minute>``: minute to run the job, an integer ranging from 0 to 59.
- ``<hour>``: hour to run the job, an integer ranging from 0 to 23.
- ``<week-day>``: day of the week to run the job, ranging from ``1`` (Monday) to ``7`` (Sunday) or ``*`` to run the job every day. 

All values have to be specified using the server's local time. For example:

- ``0 3 *``: Run the job every day at 3am local time.
- ``45 5 2``: Run the job every Tuesday at 5:45am local time.

By default, the purge job runs each day at 3am (``0 3 *``) but this can be changed with the
``cache-purge-every`` parameter:

```yaml
cache-purge-every: 0 3 *
```

It is recommended to schedule the purge task when your JMAP server is most likely to be idle.

## Vanished Ids

In order to support the ``VANISHED`` modifier of the [Quick Mailbox Resynchronization (QRESYNC)](https://www.rfc-editor.org/rfc/rfc7162) extension,
Stalwart IMAP needs to keep track of all the ids that were deleted on the JMAP server.

By default, deleted ids are kept in the cache for a maximum of 2592000 seconds (30 days). This amount can be configured in seconds
with the ``cache-removed-id-ttl`` parameter:

```yaml
cache-removed-id-ttl: 2592000 # secs
```

After a removed ID has been purged from the cache, it will no longer be returned
to clients in a ``VANISHED`` IMAP response.
