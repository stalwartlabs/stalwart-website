---
title: "Quick Start"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "cluster"
    identifier: "quick-start"
weight: 100
toc: true
---

## Introduction

Stalwart JMAP includes native clustering support and can be easily deployed in a high-availability cluster without requiring any
third party products. Under the hood, Stalwart JMAP automatically discovers new nodes
in the cluster using a gossip protocol similar to [SWIM](https://ieeexplore.ieee.org/document/1028914)
and keeps each node in sync using the [Raft consensus protocol](https://raft.github.io/raft.pdf).

Main features:

- Node autodiscovery and peer updates over encrypted [gossip protocol](https://en.wikipedia.org/wiki/Gossip_protocol).
- Adaptive [Phi accrual](https://www.researchgate.net/publication/29682135_The_ph_accrual_failure_detector) failure detection resistant to some types of network partitions.
- Replication and cluster consensus over the [Raft](https://raft.github.io/raft.pdf) protocol.
- Read-only replicas.

{{< alert icon="👉" text="Note: Although clustering is fully operational, it is currently in beta until Jepsen testing is completed." />}}


## Starting a Cluster

Running Stalwart JMAP is a cluster is straightforward thanks to its gossip protocol which automatically discovers and adds
new nodes to the cluster. The minimum number of nodes required to start a cluster is 2 but, in order to make the cluster tolerant
to failures, at least 3 nodes are required. 

There are three required parameters to start a cluster: ``encryption-key``, ``seed-nodes`` and ``rpc-advertise-addr``. These parameters
are covered in detail in the following sections but, as a quick example, let's say you have three Stalwart JMAP nodes
running on IP addresses ``192.168.0.101`` (node 1), ``192.168.0.102`` (node 2) and ``192.168.0.103`` (node 3).
To set up the cluster make sure you use the same ``encription-key`` value on all nodes, then set the ``rpc-advertise-addr`` to the server's
IP address and configure on each node the ``seed-nodes`` parameter to include one or multiple IP addresses of the other nodes in the cluster, for example:

- Node 1:
  ```yaml
  rpc-advertise-addr: 192.168.0.101
  seed-nodes: 192.168.0.102
  encryption-key: SECURE_ENCRYPTION_KEY
  ```
- Node 2:
  ```yaml
  rpc-advertise-addr: 192.168.0.102
  seed-nodes: 192.168.0.101
  encryption-key: SECURE_ENCRYPTION_KEY
  ```
- Node 3:
  ```yaml
  rpc-advertise-addr: 192.168.0.103
  seed-nodes: 192.168.0.102 ; 192.168.0.101
  encryption-key: SECURE_ENCRYPTION_KEY
  ```

And... __that's it!__ You are now running Stalwart JMAP in a high-availability cluster.

## Running a Cluster

Once the cluster has been started, clients can send JMAP requests to any member of the cluster. 
If the JMAP request only requires read access and the node receiving the request is up to date with the
leader's log, then the request is served directly from the follower node that received it.

On the other hand, if the JMAP request requires write access and, if the node receiving the request is not 
the [leader](/jmap/cluster/raft/#leader-elections) of the cluster, then the client is redirected with an HTTP ``302`` 
response to the current leader.

Similarly, when [LMTP](/jmap/configure/lmtp/) is enabled on a node, connections can be received by any
peer in the cluster. If the node receiving the LMTP connection is not the current leader, all commands
are executed on the leader over [RPC](/jmap/cluster/rpc/). No additional configurations are required.

## Example

The following example shows how to start a local cluster for testing purposes directly from the command line:

```bash
$ /usr/bin/stalwart-jmap --jmap-url=https://localhost:8081 \
                         --jmap-port=8081 \
                         --jmap-cert-path=/etc/stalwart-jmap/certs/jmap.crt \
                         --jmap-key-path=/etc/stalwart-jmap/private/jmap.key \
                         --encryption-key=SECURE_ENCRYPTION_KEY \
                         --rpc-advertise-addr=127.0.0.1 \
                         --rpc-port=7911 \
                         --seed-nodes=127.0.0.1:7912 &

$ /usr/bin/stalwart-jmap --jmap-url=https://localhost:8082 \
                         --jmap-port=8082 \
                         --jmap-cert-path=/etc/stalwart-jmap/certs/jmap.crt \
                         --jmap-key-path=/etc/stalwart-jmap/private/jmap.key \
                         --encryption-key=SECURE_ENCRYPTION_KEY \
                         --rpc-advertise-addr=127.0.0.1 \
                         --rpc-port=7912 \
                         --seed-nodes=127.0.0.1:7913 &

$ /usr/bin/stalwart-jmap --jmap-url=https://localhost:8083 \
                         --jmap-port=8083 \
                         --jmap-cert-path=/etc/stalwart-jmap/certs/jmap.crt \
                         --jmap-key-path=/etc/stalwart-jmap/private/jmap.key \
                         --encryption-key=SECURE_ENCRYPTION_KEY \
                         --rpc-advertise-addr=127.0.0.1 \
                         --rpc-port=7913 \
                         --seed-nodes=127.0.0.1:7911 &
```

