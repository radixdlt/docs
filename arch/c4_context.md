# Level 1: Radix System Context

This page implement the first "C" in the [C4Model](https://c4model.com/#coreDiagrams): the (System) **C**ontext.

All production Systems (External and Radix) and Users that are commonly found on Radix networks are introduced on this page.

![c4_context.puml](http://www.plantuml.com/plantuml/proxy?cache=no&fmt=svg&src=https://raw.githubusercontent.com/radixdlt/docs/arch/arch/c4_context.puml)

# Shards and Chunks

Described in detail in the Radix [Whitepaper v2](https://papers.radixdlt.com/tempo/v2/), shards enable Radix Networks to scale infinitely.
This is achieved by splitting up global state (Ledger) to different nodes. Each node can be part of one or more Shards slices - called Chunks.

The minimal Shard space that a nodes can serve is one Chunk. The number of Chunks served depends on the hardware configuration (CPU, MEM, I/O) of
a specific node. Each node has advanced algorithms for monitoring its own performance and dynamically - to other nodes non-deterministically - scale up and down
 the number of chunks served.

## Redundancy

Nodes within the same Shard space are redundant to each other, which means that they serve and validate the same transactions.

# Core Nodes

The first class citizens of the Radix Network. Nodes are organised in Shards. A node can serve ("be in") one or more Shard Chunks.

Core Nodes are responsible for:

* Validating all transactions that are relevant for its Chunks.
* Store all transactions that are relevant for its Chunks.
* Allow submission of atoms from Light Clients through a PubSubscribe-style API.
* Dispatch ledger events to Light Clients that are subscribed.
* Sync Ledger database with Peers in the same Shard Space.
* Gossip about transactions with Peers in any/all Shards (structured gossip; not flooding).

All nodes in the network are equal on start (same Mass). However, being a well-behaved citizen of a Radix Network, nodes gain mass 
and become more influential over time.

# Development Services

Developlment Services are deliberately left out from the Architecture, because those are only available on Test networks and will not be available on the production-grade networks. 
The services in question make it easy for newcomers to setup a test node and connect to various test networks.
For better understand, we provides the following Development Services:

* Radix NodeName Service
* Radix Node Discovery Service

*But these are cool service, why can't we use them in production?*

Hard dependency towards these services lead to centralisation, which is exactly what we are trying to prevent.
