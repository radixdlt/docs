# Cerberus FAQ

## **What are liveness and safety?**

Put simply, safety ensures that “a bad thing never happens” and liveness guarantees that “something good eventually happens” \([Seung Woo Kim](https://medium.com/codechain/safety-and-liveness-blockchain-in-the-point-of-view-of-flp-impossibility-182e33927ce6)\).  
In general, Nakamoto Consensus \(for example, Bitcoin\) favors liveness over safety in worst-case conditions - if there’s a chain that has more hashing power, it ‘wins’ and progress is always made. Alternatively, the so-called BFT class of consensus algorithms \(using multi-phase commits\) favor safety over liveness in worst-case conditions, with provable finality of transactions that cannot be reversed**.**

## **What are availability and consistency?**

According to the CAP theorem, a system can simultaneously provide only two of three guarantees - Consistency, Availability and Partition Tolerance. Consistency is understood as a status in which all nodes can access the latest version of data, availability simply means that data is available. All distributed systems must strike a balance of the compromises made with these factors.

## **What is State Machine Replication?**

State Machine Replication \(SMR\) is a general method for implementing a fault-tolerant service by replicating servers and coordinating client interactions with server replicas. Blockchains and DLTs are SMR systems that replicate the “state” of their ledger across their network of nodes.

## **What are the requirements for running a node?**

We will release detailed specifications ahead of each network release. Ultimately Cerberus is designed to allow a virtually unlimited number of node runners to participate with anything from a full server down to a Raspberry Pi, serving as much of “shard space” as possible with available processor power, storage, and bandwidth. In the early stages of the Radix network, we will have a constrained number of node runners and so they will have much more server-like requirements for processor and bandwidth to keep the network performant.

## **Is a 3 phase commit better than a 2 phase commit? Why?**

Consensus design is always a matter of tradeoffs. A 3 phase commit incurs a bit of extra messaging, but creates a more resilient, predictable consensus process. The parallelism of Cerberus allows us to make use of this advantage of 3 phase commit while not practically limiting the overall throughput of the network.

## **Is the shard space fixed?**

Yes, it is. A fixed shard space is a key enabler of the parallelism at the core of both Tempo and Cerberus.

## **What elements of Tempo stay in Cerberus?**

Massive parallelism of consensus enabled by our fixed state sharding concept is unchanged from Tempo to Cerberus. From the perspective of the Radix protocol, the application layer as well – including Atom Model and Radix Engine – also is unchanged, and is designed to work with \(and enable\) our sharded data structure**.**

## **Does Cerberus use logical clocks?**

No, logical clocks are no longer used in Cerberus.

## **What happens when part of the network goes down and wants to join later?**

In most cases, when nodes rejoin, they simply resync the transactions that have been processed while disconnected and proceed - if no more than ⅓ of the nodes \(by staked tokens\) disconnect, they cannot process any transactions while disconnected from the larger part of the network. However, if more than ⅓ of the nodes disconnect, network progress will halt \(or in Cerberus’ case, ⅓ of nodes serving each shard, for that shard\). While this situation poses no threat to the safety of transactions, this disruptive situation must be avoided by careful design of sybil protection, validator selection, and node mapping.

## **What is Cerberus’s most important contribution?**

Cerberus takes the typical single-pipe consensus process and parallelizes it across a potentially very large set of instances, or shards. It achieves this parallelization through a “braided” synchronization of consensus across shards, only when required by each transaction \(atom\).

## **What can happen to the network at large when a single shard goes rogue?**

To resolve a forked proposal in a local instance \(shard\), Cerberus reuses the Virtual QC \(Quorum Certificate - aggregated signatures of broadcasted commands\) method, explained in detail in the ‘emergent merges’ section of the whitepaper. In short, the network has the ability to automatically work around a rogue shard.

## **What are the scalability limitations of Cerberus?**

There are no fundamental theoretical limitations to the total number of independent transactions Cerberus can process concurrently in distinct shards. In that regard, Cerberus scales linearly with the number of shards. We’re still working on message complexity and threshold signatures, which are more practical - we don’t want them to bottleneck bandwidth etc. of individual nodes.

## **What are the security limitations of Cerberus?**

Cerberus requires that at least ⅔ + 1 of nodes are correct in every shard to maintain safety and liveness. If the adversary seizes controls ⅓ or more of the network, they can halt liveness and break safety. Notably, an adversary in control of the network cannot at any time revert committed transactions in correct nodes**.**

## **How does Cerberus prevent the dominance of malicious or incorrect nodes in a shard?**

As a consensus protocol, ensuring the minimum number of correct nodes falls out of Cerberus’s responsibility. Guarding influence in the network is delegated to a shardable Sybil mitigation scheme, such as Proof of Stake, as well as good network protocol design to ensure sufficient node overlap for all shards.

## **Could Cerberus work with PoW?**

No, not without significant modification. Cerberus uses quorum certificates \(QCs\) to make progress, which requires a countable voting power per node signature

