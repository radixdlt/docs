# Radix Platform

## History

\_\_[_Daniel Hughes_](https://radixdlt.com/team)_, founder & CTO of Radix DLT invented the Radix platform and ‘_[_Tempo_](../whitepapers/tempo.md)_’ - its underlying engine comprised of the consensus algorithm and data architecture._

In 2011, he discovered Bitcoin and was instantly hooked by the underlying elegance of its decentralized protocol. As he dug deep, he quickly realized its limitations and proposed various solutions on the Bitcoin Talk Forum, only to be met with criticism. Ultimately, he decided to build his own decentralized ledger that could scale to support mass market decentralized applications with millions of users simultaneously. This obsession with creating a truly scalable distributed ledger led him to build and test various suitable consensus algorithms and data architectures like blockchains, block-trees, directed acyclic graphs \(DAG\) and state channels.  
  
Having tested their limits, he found they all had a fundamental inability to scale. So he dedicated his time to develop a new architecture and [consensus algorithm](https://www.radixdlt.com/post/simple-consensus-in-radix/). Five years in R&D and after many iterations, he eventually invented ‘[Tempo](../whitepapers/tempo.md)’ - a novel distributed ledger architecture and consensus algorithm for decentralized systems, that is [sharded to scale](http://www.radixdlt.com/post/sharding-in-radix/) in an efficient, unbounded linear fashion. It uses vector clocks for generating a [partial ordering of events](http://www.radixdlt.com/videos/creating-relative-order-of-events-on-the-radix-ledger/) in a distributed system to detect and prevent causality violations.

This system is both _asynchronous_, meaning there is no block time, and [Byzantine fault-tolerant](https://www.radixdlt.com/post/simple-consensus-in-radix/), meaning that it can detect and stop false transactions and double spends within a system that anyone can join.

[Tempo](../whitepapers/tempo.md) does this by preserving the total order of events, allowing the trustless transfer of value, timestamping and other functionalities. It is a semi-structured, [shardable architecture](http://www.radixdlt.com/post/sharding-in-radix/) that limits state transfer information only to those members of the network that need it. This reduces overhead and increases performance.

It does not require large amounts of computing power \([PoW](http://www.radixdlt.com/post/what-is-proof-of-work/)\) or large amounts of capital \([PoS](http://www.radixdlt.com/post/why-proof-of-stake-is-a-powerful-alternative/)\) to secure it. It is suitable for both public and private deployments, without modification, and requires no special hardware or equipment. Combined with a vast, overlapping [shard space](http://www.radixdlt.com/post/what-is-sharding/), the scalability of Radix is only constrained by the number of nodes operating within the network.

{% hint style="success" %}
**Tip:** discuss the Radix History on our [developer forum](https://forum.radixdlt.com/t/the-radix-history/34).
{% endhint %}

## What is Radix?

Radix is a new platform, like Bitcoin or Ethereum, but scalable and easy to build on. Instead of using blockchain, we started from scratch with an entirely new design so that every person and device in the world could use the Radix ledger without centralization or compromise. We created [building blocks](../architecture/atom-structure.md) for developers, making applications, tokens, and coins as easy to deploy as possible. It’s fast, it works, and our [test net](https://explorer.radixdlt.com/) is already live.

It's a speedy alternative to [blockchains](https://www.radixdlt.com/post/blockchains-are-broken-here-is-why) and [DAGs](https://www.radixdlt.com/post/dags-dont-scale-without-centralization). It uses both the [passage of logical time](https://youtu.be/wfsZuN6NaJo) and database sharding to create an immensely secure and scalable system for the storage and accessing of immutable data and decentralized logic.

Decentralized Ledger Technology \(DLT\) offers a powerful new platform for the storage, verification, and use of digital identity. One that is cryptographically strong, very difficult to hack and steal, and one in which the infrastructure it works on is _antifragile_; a term that means it is resilient even in the event of a massive disruption. 

Prior to the creation of Radix, the predominant technology for building and deploying DLT was blockchain, a technology that allows many computers to agree on a single version of events, without any center to the system.

This is a powerful concept as there is no single place to attack or disrupt. It provides certainty of data integrity, and trust in the transactions conducted using it. But it has a major drawback - [it does not scale very well](https://www.radixdlt.com/post/blockchains-are-broken-here-is-why).

This has not been a significant problem for the relative niche use cases that it has been used for so far. However as the applications that can be built using the technology have broadened, the technical limitations have become clear. Specifically, [the block size limit](https://youtu.be/sW8nWeUnkK0), which means the maximum throughput a blockchain can cope with is around 500 transactions per second. If you compare that to Visa, which is handling 2,000 transactions per second, blockchain is clearly not ready for mainstream applications.

Limitations of current decentralized ledger solutions:-

1. [They cannot scale](http://www.radixdlt.com/videos/introduction-to-radix-blockchain-scaling-issues/) to meet demand
2. A consensus is inefficient and resource intensive
3. Networks are prone to miner centralization
4. Smart Contracts are complex and expensive to use
5. Volatile crypto currencies cannot be used as cash

The Radix ledger eliminates all of these limitations.

It has been the result of 6 years of research and development into many unsolved computer-science problems. It's a decentralized ledger which is redundantly replicated but without the drawbacks of inefficient uses of large amounts of power, and blockchain’s inability to scale to millions of simultaneous users, without significant compromise or centralizing.

{% hint style="success" %}
**Tip:** [discuss this topic on our developer forum](https://forum.radixdlt.com/t/what-is-radix/35).
{% endhint %}

## How is Radix different?

For a decentralized protocol to be mass adopted, it should be designed to be **fast**, **scalable**, **efficient** and **secure,** but without compromising on **security** or **decentralization.** It is also critical to incentivize network participants with a low volatile crypto currency that eventually becomes **stable** and thus **usable** as a peer-to-peer version of electronic cash, as originally envisioned by **Satoshi Nakamoto**. Finally, it is crucial to build a **developer-friendly** platform that everyone can rely on. 

To achieve this, Radix created [Tempo](../whitepapers/tempo.md) a combined distributed ledger architecture and consensus algorithm that is massively sharded to scale to 7 billion people and 30 billion devices simultaneously. 

Radix is both asynchronous Byzantine fault tolerant \(**aBFT**\) to guard against 51% [network split attacks](http://www.radixdlt.com/post/what-are-network-splits/) and asynchronous Byzantine fault detective \(**aBFD**\) to detect malicious activity even with [99% bad actors in a system](https://www.radixdlt.com/post/simple-consensus-in-radix/) \(or on a shard\).   
  
It does this using the theory of sharding and the [passage of logical time](https://youtu.be/wfsZuN6NaJo). 

{% hint style="success" %}
**Tip:** [discuss this topic on our developer forum](https://forum.radixdlt.com/t/how-is-radix-different-simplified/36).
{% endhint %}

## How does Radix work? \(Simplified\)

{% hint style="info" %}
_Radix is built on a combined architecture and consensus algorithm called Tempo, which is fully outlined in the_ [_Tempo white paper_](http://bit.ly/2ADK8LA)_._
{% endhint %}

[Tempo](../whitepapers/tempo.md) aims to create a secure and reliable consensus on a decentralized public ledger. To do this, it must be:

* Asynchronous - any process on the network can start or end whenever it needs to - e.g., it does not need to wait for a “block” to be mined.
* Highly concurrent - many processes can be done simultaneously, without bottlenecks - e.g., there is not a limit to the number of transactions that can be fit into a “block”
* Scalable - there is not an upper limit restricting the total throughput of the network, such as the limits CAP Theorem puts on Blockchains and DAGs.

To achieve this, Tempo relies on two simple ideas and one logical leap:

1. The use of a basic digital clock called a “[logical clock](https://www.youtube.com/watch?v=wfsZuN6NaJo)”
2. Telling your neighbors what has been happening, a process called “[Gossip](https://www.youtube.com/watch?v=9kPyyOIn6Bo)”
3. To stop double spends across shards, your shard address should be based on your wallet public key

A [logical clock](../glossary.md#logical-clock) is a counter that is incremented by 1 every time something new happens. On Radix, “_something new_” is when one Node speaks to another in the Radix network. This gives every Node their own relative time, based on network activity, and one they can use to create a simple order of events.

[Gossip protocols](http://www.radixdlt.com/videos/updates-about-new-events-via-gossip/) are well established in computer science and are one of the fastest ways in which information can be reliably shared across a network. It works simply by a [Node](../glossary.md#nodes) choosing a couple of other [Nodes](../glossary.md#nodes) to tell something new to, and they, in turn, telling two other Nodes, and so on and so on. This causes information to spread at an exponential rate.

On [Tempo](../whitepapers/tempo.md), we add the Node [logical clocks](../glossary.md#logical-clock), signed by the Nodes in question, to the gossip they are spreading around the network. This allows everyone to see both new information, and at what logical clock time that information was seen by other members of the net.

To allow high scalability a [Tempo](../whitepapers/tempo.md) ledger is split into a vast shard space, allowing a massive degree of concurrency. To avoid a double spend across any of the shards, the shard a wallet lives on is determined by its public key. This makes sure that any spend from a wallet will always start on the same shard.

When combined with logical clocks and [gossip protocols](http://www.radixdlt.com/videos/updates-about-new-events-via-gossip/), the Tempo consensus algorithm will always find the total ordering of related events, allowing double spends to be quickly detected and ignored.

{% hint style="success" %}
**Tip:** [Discuss this topic on our developer forum](https://forum.radixdlt.com/t/how-does-radix-work/59?u=angad).
{% endhint %}

## Why should I develop with Radix? 

Radix is used by decentralized applications that demand fast transactions with near-instant finality. Mass market applications like payment networks, games, marketplaces, and exchanges are some examples.

## Platform Features

### Fast, Scalable & Cheap

* Asynchronous, ~0.2 transaction time, &lt;10 seconds transaction finality time. Transactions settle as fast as fiat transactions, due to the fact that by design Radix groups related transactions together.
* Massive sharded to scale linearly with no upper bound - solves the trilemma of decentralization
* Negligible transaction fees projected to cost up to 1¢

### Efficient & Secure

* A secure algorithm that achieves consensus with even [99% malicious nodes on a shard](https://www.radixdlt.com/post/simple-consensus-in-radix/) 
* Reduces energy footprint by 98% as the algorithm is not based on Proof of Work

### Truly Decentralized and Fair

* Lowers the entry barrier by making simple how to deploy a node
* Node runs on standard commercial hardware
* Avoids network centralization by not requiring high computing power or capital to secure the network
* Does not discriminate low powered devices by enabling them to partially support the network as per their resource capacity and providing an equal chance of earning rewards. 

### **Price Stable Token**

* Low Volatility Token \(XRD\) - enables a true cryptocurrency that protects consumers and merchants by dampening volatility and preserving a long-term price value curve.

### Miscellaneous

* Private and Public Network deployments use the same technology.
* Constraints Machine allows for validating state transitions instead of computing them
* Modular - allows for multiple layer constraints and development opportunities
* High-level API - allows for building most DLT use-cases without the use of smart contracts
* All token types are first class citizens - allows paying for services without having XRD tokens

### **Client Libraries**

* [Java library]()
* [JavaScript library]()
* [Kotlin library](../../develop/kotlin-client-library/)

### **Radix Smart Cards**

A [decentralized smart/debit card](../use-cases/card-payment-system.md) compatible with existing point-of-sale systems for enabling merchants to accept Radix assets faster and helping users spend them anywhere.

## Try it out

The Radix platform is now in our Alpha stage of development, with a live Alpha test network that you can connect to and try building against. If you're a developer, find out more here: [https://www.radixdlt.com/developers](https://www.radixdlt.com/developers)

{% hint style="success" %}
**Tip:** start building with our GitHub projects: [https://github.com/radixdlt/](https://github.com/radixdlt/)
{% endhint %}

All our libraries are open source. The core engine code is not currently open-sourced, as we have not yet launched the main net. However, once launched, all the Radix code \(including libraries, Engine and Tempo\) will be 100% open source. We believe that to build a platform for the world to build on, it must be an open one.

In the [next section](tempo.md), we discuss the design philosophy of [Tempo](../whitepapers/tempo.md) and how it works to enable a fast, scalable and straightforward DLT platform to build on.

## Join the Radix Community

* ​[Telegram](https://t.me/radix_dlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [hello@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.

