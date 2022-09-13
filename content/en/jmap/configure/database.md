---
title: "Database"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "database"
weight: 203
toc: true
---

## Overview

Stalwart JMAP stores all metadata and settings in [RocksDB](http://rocksdb.org/), a high performance embedded database 
for key-value data. RocksDB is used in production systems at various web-scale enterprises including Facebook, Yahoo! and LinkedIn.
It has also been adpoted as storage backend by popular SQL and NoSQL databases such as Apache Cassandra, CockroachDB and MySQL.

Data integrity is a priority in Stalwart JMAP and, even though RocksDB can be fully trusted with your information, e-mail messages are 
stored separately from metadata, directly in the file system to allow quick recovery in case of catastrophic failure.

## Configuration

The path to where both RocksDB tables and blobs are stored is configured with the ``db-path`` parameter and its default
value is ``/usr/local/stalwart-jmap/data``. Under this path, RocksDB tables are stored under the ``idx`` directory and blobs
under ``blobs``.

Example:

```
db-path: /usr/local/stalwart-jmap/data
```

## Blobs

Binary large objects (blobs) are stored, depending on their type, either on RocksDB or directly in the filesystem.
Metadata blobs such as parsed e-mail headers and full text term indexes are stored in RocksDB while e-mail messages
are stored in the file system using a nested directory structure.

The following blob storage parameters can be configured:

- ``blob-nested-levels``: Number of nested subdirectories to use to distribute blobs in the file system. For example a value of 1 will store blobs under ``<db-path>/blobs/<l1>`` while a value of 3 will store blobs under ``<db-path>/blobs/<l1>/<l2>/<l3>`` where l1, l2, ..., ln is the hexadecimal representation of an 8 bits hash. The default value is 2.
- ``blob-min-size``: For metadata blobs, the minimum size in bytes that RocksDB will use to store a blob outside the key-value store. For example, a value of 1024 will keep blobs under 1024 bytes in the key value store and any blobs larger than 1024 bytes will be stored in the file system. Defaults to 16384 bytes.
- ``blob-temp-ttl``: Time in seconds after which a temporary blob is purged. Temporary blobs are those uploaded via JMAP that have not yet been linked to any JMAP object. Defaults to 3600 seconds (1 hour.)

Example:

```
blob-nested-levels: 2
blob-min-size: 16384 # bytes
blob-temp-ttl: 3600 # seconds
```

## Housekeeper

Housekeeper is a job scheduler service in charge of running database maintenance tasks such as optimizing read workloads, consolidating 
information in the database, and removing expired or deleted entries. Housekeeper runs these different
tasks at intervals that are configured using a simplified [crontab](https://en.wikipedia.org/wiki/Cron) expression.

Housekeeper jobs are scheduled using the format:

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

It is recommended to schedule housekeeping jobs when your JMAP server is most likely to be idle.

### Compaction

The compaction job triggers a database compaction on RocksDB in order to consolidate newly added data and optimize
read workloads.

It is configured with the ``schedule-compact-db`` parameter and the default schedule is to run every day at 4 am (``0 4 *``). 

Example:

```
schedule-compact-db: 0 4 * # min hour week-day
```

### Snapshots

The log snapshot job creates a snapshot containing old Raft log entries and JMAP States. Raft logs are used in distributed environments while JMAP states are
used to keep track of changes to JMAP objects.

The snapshot schedule is configured with the ``schedule-snapshot-log`` parameter and the default setting is to run every day at 3:45 am (``45 3 *``). 
Additionally, this job takes the ``max-changelog-entries`` parameter which determines the maximum amount of recent log entries to
keep and all previous entries exceeding this limited are added to the snapshot (defaults to 10000 entries).

Example:

```
schedule-snapshot-log: 45 3 * # min hour week-day
max-changelog-entries: 10000
```

### Blobs

Stalwart JMAP keeps track of the number of JMAP objects linked to a particular blob and when this number drops to zero, 
the blob is tombstoned (marked for deletion). The blob cleanup job takes care of removing these tombstoned blobs as well as
all expired blobs (see ``blob-temp-ttl`` above) from the file system. 

The job schedule is configured with the ``schedule-purge-blobs`` parameter and the default setting is to run every day at 3:30 am (``30 3 *``). 

Example:

```
schedule-purge-blobs: 30 3 * # min hour week-day
```

### Accounts

Since removing all the information linked to a user account from the database is a relatively expensive operation, this
process is performed in batches from a housekeeper task. Once an account is deleted, the user won't be able to access it
anymore but all their data won't be deleted until later. The purge accounts task permanently removes from the database all information
linked to the accounts that had been scheduled for deletion.

The account purge schedule is configured with the ``schedule-purge-accounts`` parameter and the default setting is to run every day at 3 am (``0 3 *``). 

Example:

```
schedule-purge-accounts: 0 3 * # min hour week-day
```

