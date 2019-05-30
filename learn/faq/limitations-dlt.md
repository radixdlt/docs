# Limitations of DLT

## What is Distributed Ledger Technology?

Distributed Ledger Technology refers to the technological infrastructure and protocols that allows simultaneous access, validation and updating records in an immutable manner across a peer to peer decentralized network of computers spread across multiple entities or locations.

More commonly known as the blockchain technology, DLT which was first introduced by bitcoin and subsequently popularized by Ethereum is now a buzzword in the technology world given its potential across industries and sectors. In simple words, it is all about the idea of a ‘"decentralized" network against the conventional "centralized" mechanism, and is deemed to have far-reaching implications on sectors and entities that have long relied upon a "trusted third-party."

## What problems does it solve?

Contracts, transactions, and the records of them are among the defining structures in our economic, legal, and political systems. They protect assets and set organizational boundaries. They establish and verify identities and chronicle events. They govern interactions among nations, organizations, communities, and individuals. They guide managerial and social action. And yet these critical tools and the bureaucracies formed to manage them have not kept up with the economy’s digital transformation. They’re like a rush-hour gridlock trapping a Formula 1 race car. In a digital world, the way we regulate and maintain administrative control has to change.

Distributed Ledgers promise to solve these problems. The technology at the heart of bitcoin and other decentralized protocols, the blockchain, is an open, distributed ledger that can record transactions between two parties efficiently and in a verifiable and permanent way. The ledger itself can also be programmed to trigger transactions automatically.

Cryptocurrencies such as Bitcoin and Ethereum are beginning to attract the attention of mainstream media thanks largely to their massive rise in value over the past year. The world is starting to realize the potential of blockchains. It has become increasingly evident that decentralized networks, running on distributed ledgers and token economics, will radically change industries over the coming decades.

In particular, it will disrupt three key areas;

1. How we own things: like companies, buildings, and contracts
2. How we organize things:- like governments, shareholder voting, and marketplaces
3. How we value things:- like human capital, intellectual property, and rights of use

Despite these innovations, the low scalability of the current breed of technology, and the high volatility of their value have delayed adoption, either for mainstream applications or as a useful technology.

## What problems do current distributed ledger architectures have?

Current distributed ledger architectures like [Blockchains](http://www.radixdlt.com/videos/introduction-to-radix-blockchain-scaling-issues/) and [Directed Acyclic Graphs](http://www.radixdlt.com/videos/radix-dag-scaling-issues-aims-of-radix/)  do not  live up to the hype. They do not scale without adding some form of central authority. It is difficult and expensive to build and deploy secure applications. They are difficult for consumers to use, with a steep learning curve for even simple tokens. They are inefficient and consume massive electric power, are prone to miner centralization, and the high volatility of their token/coin have delayed adoption, either for mainstream applications or as a useful currency.

### Existing protocols cannot scale to meet demand

Present blockchain based distributed ledgers do not scale well under load. This is because they are vertical architectures where all nodes participating in the network are required to have the complete global state of the system. The global state requires that all events are delivered and replicated on all nodes - this global requirement to always stay in sync gives an upper limit to the total throughput possible - often referred to as the CAP Theorem limit - of around 500 transactions per second, assuming no other limits.

* Late last year crypto kitties managed to completely jam and halt the Ethereum network
* With 100,000 users and 20,000 kitties sold the network slowed from seconds to minutes to process transactions
* Transactions settlement takes anything between 2.5 minutes to 10 minutes.
* Directed acyclic graphs \(DAGs\) such as IOTA and Hashgraph offer scaling but at the expense of decentralization

### Issues with blockchains

{% hint style="info" %}
[Blog post on why blockchain are broken](https://www.radixdlt.com/post/blockchains-are-broken-here-is-why)
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=sW8nWeUnkK0" caption="The first in a series of videos to describe how blockchains work and what are their limitations" %}

### Issues with directed acyclic graphs \(DAGs\)

{% hint style="info" %}
[Blog post on why DAGs don't scale without centralization](https://www.radixdlt.com/post/dags-dont-scale-without-centralization)
{% endhint %}

{% embed url="https://www.youtube.com/watch?v=36DRDd8mh0o" caption="A brief introduction to why Directed Acyclic Graphs do not scale without centralization, as well as an introduction to Radix and our aims with the technology." %}

### Smart contracts are complex & expensive

* Using blockchain for smart contracts requires hiring expensive security experts and external validation before they can be deployed
* Both the frozen Parity Multi-sig wallet and the fall of the DAO caused the loss of funds in excess of $300M. This was code written by the founders of Ethereum.

### Networks are prone to miner centralization

#### **Proof of Work\(PoW\):**

Every ten minutes, bitcoin miners compete with each other to mine the next block of transactions. The first miner to do so wins and gets all of the block rewards. Work done by every other miner is wasted. In a winner takes all \(block reward + transaction fees\) race each period, there is only one way to improve your chances of being successful: increase your hash power. This competitive nature of the Hashcash Proof of Work\(PoW\) algorithm leads to three results:

1. It becomes uneconomic for small/low power devices to participate
2. The pooling of resources
3. The specialization of hardware

This eventually leads to the re-centralization of the network, where only very large miners have any chance of earning mining rewards. At the extreme end, this leaves the possibility of collusion, abuse, and censorship.

#### **Proof of Stake\(PoS\):**

While success in Proof of Work is determined by who has the most hashing power, Proof of Stake is determined by whoever holds the most coins.

This essentially means that the network is available to be purchased by the highest bidder. With a very small group already holding a majority of coins on many Proof of Stake networks, this puts the control of the network immediately into the hands of a small, rich elite. Not massively dissimilar to banking.

The alternative would be for smaller stakeholders to pool together and move all of their wallets to a central service to share running costs reducing the per unit cost of the operation.This introduces significant security risk not only to hacking but to outside jurisdictional forces such as censorship, regulation, and taxation.

One way or the other, PoS will lead to centralization the same way PoW does due to economies of scale where the rich get richer, faster. Although PoS reduces the energy cost to run the network to a fraction of what PoW requires, it does not solve the centralization problem.

#### **Delegated Proof of Work/Proof of Stake**

Some cryptocurrency architectures introduce the requirement for trust in a trustless network by using coordinator nodes.

A coordinator node has three principal problems:

1. All other Nodes must trust the coordinator nodes to be honest; making the % of the network you need to corrupt much smaller
2. Much more DDoS susceptible: attack the coordinators, bring down the network
3. Frequently more reliant on a single company/entity to keep the network running. Not autonomous/independent of their creators

This addition of trusted parties can give a significant performance boost, but at the cost of making the network far more vulnerable than it would otherwise be.

### **Inefficient Consensus Algorithms**

#### **Energy Wastage**

Proof of Work is an incredibly expensive and inefficient way of reaching consensus and creating security in a trustless network. This is because hashing to find the largest number of leading zeros before anyone else \(“mining”\) ends up consuming an ever-increasing amount of electrical power as more and more computers compete to mine the next block.

This process already consumes more electricity than the entire power consumption of Ireland. It is wasteful, and at a time when we are looking for ways to save energy and decrease the environmental impact we have on the world, it is a definite step in the wrong direction.

#### **Processing Inefficiency**

As the network gets bigger Smart Contract Execution on an un-sharded DLT \(such as the current Ethereum network\) becomes more expensive. This is because each Node on the network is required to execute every line of code in every Smart Contract submitted with sufficient gas.

As more Nodes join the network the lower the chances any given Node has to be rewarded with the gas fee for executing a Smart Contract. This means that each Node must anticipate needing to run a larger and larger number of Smart Contract before successfully earning a reward. As a result, it is likely that as the network grows, the higher the gas fee is likely to become once the mining rewards are taken into account.

### **Price Stability**

For an object to become a currency, it has to fulfill three roles: a medium of exchange; a unit of account; a store of value. Although DLT technology has enabled trustless transactions over the internet, volatility has delayed their adoption as a medium of exchange and a unit of account. For cryptocurrencies to be adopted widely, they must equal or better the certainty of future purchasing power that fiat currently offers. A low-volatility coin is an asset designed to be price stable over time. This makes it suitable for short-term and medium-term use as a unit of account, a medium of exchange and, a store of value.

The Radix Stable Token is a proposed relatively price stable cryptocurrency currently in R&D. It will sit as a module on top of the Radix Tempo protocol and will be controlled by an algorithmic monetary policy of expanding and contracting coin supply. Full details will follow in a future white paper.

It is clear, mass adoption of cryptocurrencies is impossible without a means of achieving both transactional **scalability** and **price stability** - Radix is designed to solve all of these issues. [Learn how Radix eliminates all of these limitations as we](../platform/) dive deep into the Radix platform and how it works under the hood.

{% hint style="info" %}
[Share your problems with current decentralized ledgers and how can they be improved on our developer forum](https://forum.radixdlt.com/t/what-problems-do-current-deceentralized-ledgers-have/54)
{% endhint %}

## Join the Radix Community

* [Telegram](https://t.me/radixdlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [hello@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.

