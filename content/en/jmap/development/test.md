---
title: "Tests"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "development"
    identifier: "test"
weight: 602
toc: true
---

## Overview

Stalwart JMAP includes a series of tests that ensure, among other things, data integrity
and protocol compliance.
The following subsections cover the different tests available on Stalwart JMAP.
Before running these tests, make sure you have Rust installed by following the instructions
in the [compile section](/jmap/development/compile).

## Base tests

The base tests perform basic testing on different functions across the Stalwart JMAP
code base. To run the base test suite execute:

```bash
cargo test --all
```

## Database tests

The database test suite performs a range of tests on the embedded key-value store such as:

- Concurrent insertions.
- Database queries.
- Concurrent blob insertions and blob expiration.
- Log compactions.

To run the database test suite execute:

```bash
cargo test store_tests -- --ignored
```

## Core tests

The core test suite performs authorization, authentication and JMAP protocol compliance tests such as:

- Access Control Lists (ACL) enforcement.
- Authorization and Rate Limiting.
- Event Source (JMAP Core).
- OAuth authentication.
- Push Subscriptions (JMAP Core).
- WebSockets (JMAP over WebSocket).

To run the core test suite execute:

```bash
cargo test jmap_core_tests -- --ignored
```

## Mail tests

The mail test suite performs e-mail and JMAP Mail protocol compliance tests such as:

- ``Email/*`` functionality and compliance.
- ``Mailbox/*`` functionality and compliance.
- ``Thread/*`` functionality and compliance.
- ``EmailSubmission/*`` functionality and compliance.
- ``SearchSnippet/get`` functionality and compliance.
- ``VacationResponse/*`` functionality and compliance.
- Message thread id creation.
- LMTP message ingestion.

To run the mail test suite execute:

```bash
cargo test jmap_mail_tests -- --ignored
```

## Stress tests

The stress test suite generates concurrent random insert, get, query, update and delete
operations on different JMAP datatypes to ensure data integrity. To run the stress test suite execute:

```bash
cargo test jmap_stress_tests -- --ignored
```

Another way of stress testing Stalwart JMAP is by using the [IMAP stress test tool](/imap/development/test).


## Cluster tests

The cluster test suite starts a Stalwart JMAP cluster consisting of five nodes and performs the 
following tests:

- Distributed insert, read, update and delete operations.
- Distributed e-mail thread merges.
- Read replicas.
- LMTP ingestion over RPC.
- Raft elections.
- Raft log conflict resolution.

To run the cluster test suite execute:

```bash
cargo test cluster_tests -- --ignored
```

## Cluster fuzz

The cluster fuzz test suite starts a Stalwart JMAP cluster consisting of five nodes and attempts
to corrupt the cluster state and/or its data by randomly performimg the following operations:

- Crash current leader.
- Crash follower.
- Start offline follower.
- Start all offline nodes.
- Insert record.
- Update record.
- Delete record.

Please note that this test runs on a loop and does not stop unless a leader fails to be elected
or a data corruption problem is found. If something goes wrong while running the tests, 
all the actions that were executed up to that point will be printed to screen as JSON structure.
This JSON dump can be later used to reproduce and eventually debug the problem.

To run the cluster fuzz test suite execute:

```bash
cargo test cluster_fuzz -- --ignored
```

## Cluster Jepsen tests

Support for Jepsen testing is planned and in the roadmap. Until then, the clustering module will remain in beta.

## JMAP test suite

Compliance with JMAP protocol may also be tested using Fastmail's JMAP-TestSuite:

- Clone the JMAP TestSuite repository:
    ```bash
    git clone https://github.com/stalwartlabs/jmap-test-suite.git
    cd jmap-test-suite/
    ```
- Install the recommended Perl dependencies:
    ```bash
    cpanm --installdeps .
    ```
- Run one of the tests, for example:
    ```bash
    JMAP_SERVER_ADAPTER_FILE=eg/stalwart.json perl -I<PATH_TO_PERL_LIB> -I lib t/basic.t
    ```

## Fuzz

To fuzz Stalwart JMAP server with `cargo-fuzz` execute:

```bash
 $ cargo +nightly fuzz run jmap_server
```
