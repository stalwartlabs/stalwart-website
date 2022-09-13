---
title: "Gossip Protocol"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "cluster"
    identifier: "gossip"
weight: 502
toc: true
---

## Overview

Gossip protocol is a network protocol that allows distributed systems to disseminate information about their
members in a decentralized manner, similarly to how epidemics spread. Stalwart JMAP uses a gossip protocol to 
manage cluster membership, broadcast updates and detect failures in other peers. The gossip protocol used
in Stalwart JMAP is similar to the
[Scalable Weakly-consistent Infection-style process group Membership](https://ieeexplore.ieee.org/document/1028914) (SWIM) protocol
but modified to use [Phi accrual failure detection](https://www.researchgate.net/publication/29682135_The_ph_accrual_failure_detector) instead
of the randomized probing protocol proposed by the SWIM paper.

To detect failures and membership changes in the cluster, the gossip protocol sends periodically *ping requests* to random nodes. 
Each one of these requests contain all the information that the sender knows about each member of the cluster. 
A typical *ping* gossip packet includes an entry for each member node containing details such as:

- __Peer Id__: Unique identified of a peer.
- __Epoch__: A lamport timestamp that each node increments before sending a new gossip packet.
- __Generation__: A hash representing the current state of the node.
- __Last log term__: Raft term of the last known entry.
- __Last log index__: Raft index of the last known entry.

All gossip packets exchanged by the cluster are transmited over UDP. However, if a node detects that the *generation* number for a peer is higher than the one stored, a full
state synchronization is done with that peer over TCP.

## Failure Detection

[Lamport clocks](http://lamport.azurewebsites.net/pubs/time-clocks.pdf) are used to determine the order of packets
sent by a node. The *epoch* numbers included in a gossip packet are lamport timestamps that tell a node whether the information received from given peer is
more recent than the one stored or if it is older and has to be discarded. 

Whenever a higher *epoch* value is found, the heartbeat counter for that node is incremented and a new φ value is calculated. 
Details of how φ is calculated can be found in the original [paper](https://www.researchgate.net/publication/29682135_The_ph_accrual_failure_detector).
The φ value determines whether a node has to be considered as __alive__, __suspected__ or __offline__. During normal operations, a peer is considered __alive__ 
as long as the φ value stays under ``5``. A φ value between ``5`` and ``9`` marks the node as __suspected__.
If φ exceeds ``9`` the node is marked as __offline__ and a new [raft election](/jmap/cluster/raft) is started.

The interval at which gossip ping packets are sent can be configured in milliseconds with the ``peer-ping-interval`` parameter
and its default setting is to send ping requests every 500 milliseconds. Example:

```
peer-ping-interval: 500 # ms
```

## Node Discovery

Each time a Stalwart JMAP node is started, it sends a *join request* over the gossip protocol to one or multiple **seed nodes**.
These seed nodes are not a special type of node, any member of the cluster can act as a seed node.
The *join request* includes the local peer ID and the port number where the gossip service is listening for packets.
If the packet can be successfully decrypted, the receiving node replies with a *join reply* packet authorizing the peer to join the cluster.
Once the new peer has been authorized, it performs a full state synchronization over TCP with the other peers and finally joins the cluster as a Raft member.

The list of seed nodes to use is configured with the ``seed-nodes`` parameter. It contains the list of IP addresses where to send
*join requests* when joining the cluster. By default the same gossip port number configured locally is used to reach other nodes, but it can be changed
by adding ``:<port>`` after the IP address. 

Example:

```
seed-nodes: 192.168.0.100; 192.168.0.101:11333; 192.168.0.102
```

During the initial synchronization phase, peers exchange the IP addresses where their [RPC](/cluster/rpc/) services 
are listening for connections. This IP address is the network address where the node can be reached by other peers and
it is configured with the ``rpc-advertise-addr``. For example:

```
rpc-advertise-addr: 192.168.0.100
```

## Encryption

Even though no sensitive information is transmitted over the gossip protocol, all gossip
packets are encrypted with a symmetric key known only to the members of the cluster.

This encryption key is derived using the [BLAKE3](https://en.wikipedia.org/wiki/BLAKE_(hash_function)#BLAKE3)
cryptographic hash function from the main encryption key and a context value.
Using this key, all UDP gossip packets are encrypted with the
nonce misuse-resistant authenticated encryption algorithm [AES-256-GCM-SIV](https://www.rfc-editor.org/rfc/rfc8452).

The encryption key is configured with the ``encryption-key`` parameter and the exact same key
has to be used by all members of the cluster.

Example:

```
encryption-key: SECURE_KEY
```

It is recommended to generate an encryption key using OpenSSL. Alternatively, in Linux systems the command
``LC_ALL=C tr -dc '[:alpha:]' < /dev/urandom | head -c 64`` can be used if OpenSSL is not available.
