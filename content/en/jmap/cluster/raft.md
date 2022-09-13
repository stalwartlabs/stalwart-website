---
title: "Consensus Protocol"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "cluster"
    identifier: "raft"
weight: 503
toc: true
---

## Overview

Consensus is a problem in fault-tolerant distributed systems in which multiple servers have to agree on values
in order to provide information [consistency](https://en.wikipedia.org/wiki/CAP_theorem).
The consensus protocol used in Stalwart JMAP is a modified version of [Raft](https://raft.github.io/raft.pdf), 
which was adapted to share log information with JMAP states and efficiently reach consensus on large objects such as e-mail messages. 

Details on how the Raft consensus algorithm works can be found at the [official Raft website](https://raft.github.io/) and
[The Secret Lives of Data](http://thesecretlivesofdata.com/raft/).

## Leader Elections

Raft requires that at any given time only a single node can write entries to the replicated log. This node is known as the __leader__
and is elected through a voting mechanism in which all cluster members participate. Each leader election has an associated *term* number
and is performed by sending vote requests to all peers in the cluster. A vote request includes the state of the peer's replicated logs. 
The peer with with the most recent version of the logs receives the majority of the votes and becomes the new leader
for that term. This of course is a simplification of the entire process, for a detailed explanation refer to the Raft [paper](https://raft.github.io/raft.pdf).

Leader elections have to be completed within a specific timeframe. If no leader is elected within that time, the *term* number is increased
and a new election is started. This process is repeated until a new leader is elected.

The default amount of time to wait for a leader to be elected is 1000 milliseconds (1 second) and it can be configured with the ``raft-election-timeout`` parameter. 
For example:

```
raft-election-timeout: 1000 # ms
```

## Commits

Before an entry can be committed to the Raft replicated log, it has to be added to the logs of the majority of the cluster members.
When the leader receives an update from a client, it propagates the change to all followers and waits for a brief period of time until
the majority of followers confirm that they have received the update. If no confirmation is received from the majority of followers,
the request is rejected with a ``serverPartialFail`` JMAP error.

The amount of time to wait to commit a raft log entry is configured with the ``raft-commit-timeout`` parameter. The default value is
1000 milliseconds (1 second).

Example:

```
raft-commit-timeout: 1000 # ms
```

## Batches

Raft requests are sent over [RPC](/cluster/rpc/) and grouped in batches to optimize network resources.
The size in bytes of each batch is configured with the ``raft-batch-max`` parameter and the default value
is 10485760 bytes (100 MB).

Example:

```
raft-batch-max: 10485760 # bytes
```

