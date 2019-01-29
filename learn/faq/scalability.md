# Scalability

## FAQ

This document is a compilation of questions that have been asked by the Radix community members across different channels.

### How many transactions per second can Radix scale up to?

Our current alpha network has scaled up to 25,000 tx/s with only 16 to 20 nodes and **without sharding**. Our team recently tested speeds up to 80,000 tx/s on the same alpha network. We are focusing on optimizing the node first, before proving our theory of unbounded scalability with sharding. Keep an eye on our [twitter](https://twitter.com/radixdlt) and [telegram](https://t.me/radixdlt) for milestone announcements. Check our [roadmap here](https://radixdlt.com/roadmap).

When the network is not being stress tested, we do a sustained transaction throughout of about 2,500 tx/s. [Check our explorer here](https://explorer.radixdlt.com/).

### What is linear scalability?

Linearly scalability is the ability of Radix to process more transactions per second as more nodes join the network. This property helps the network scale in an unbounded, linear fashion, where every new node increases the overall throughput of the network. Increasing the amount of nodes have little to no detrimental effects on pre-existing Nodes and thus the scalability is deemed linear and unbounded.

### How does Radix achieve linear scaling?

Radix is massively sharded to scale linearly. [Read more about our design philosophy here](../platform/tempo.md#scaling-distributed-ledgers).

### What is sharding and how does it help Radix scale?

Sharding is way to distribute network load by cutting up the distributed database into multiple manageable parts. We wrote a ELI5 blog post on [what is sharding](https://www.radixdlt.com/post/what-is-sharding) and an [introduction to sharding in Radix](https://www.radixdlt.com/post/sharding-in-radix).  
  
[Read about our protocol design philosophy](../platform/tempo.md#scaling-distributed-ledgers) and how sharding will help us scale to millions of devices.

### Can nodes maintain parts of the ledger?

Yes. The Radix ledger is divided into 18.4 quintillion shards. Depending on your node capacity \(based on CPU and bandwidth\), the node can scale down to serve only some shards/parts of the ledger. There is also a high degree of incentive for nodes to maintain under-served shards as the more shards low node count shards they maintain \(serve\) the higher the probability of getting fees.Thus every node maintains as many shards as it is able to; if a shard becomes too heavy for the node, it will automatically drop that shard. The more shards a node services, the more likely it is to get fees.This allows any device to be a useful participant of the network creating the trust layer for the internet of things. 

### How do you ensure high availability for each shard?

Incentives in the Radix network are designed such that under served shards become an opportunity for node operators. Less nodes on a shard increases the possibility of earning more rewards for validating transactions. This allows the market to naturally gravitate towards supporting as many shards as possible, especially those which are under served. Radix is designed to provide a high degree of redundancy due to its massive shard space of 18.4 quintillion shards \(2^64\).   
  
At the start, all nodes will hold all shards, and pretty much every node will store all the transactions provided enough time has passed for gossip.The more nodes that have been in a shard, the more secure a shard history is due to the number of redundant copies of the shard information being maintained. 

It is also possible for nodes to leave a shard, come back later and re-affirm history. This makes any attack vector, not just all the nodes in a shard now but all the nodes that have ever witnessed the event you are trying to change.

As discussed before there is also a high degree of incentive for nodes to maintain under-served shards as the more shards low node count shards they maintain \(serve\) the higher the probability of getting fees.

For shard selection incentives, see: [https://papers.radixdlt.com/incentives/\#shards](https://papers.radixdlt.com/incentives/#shards)

### Is the number of shards constant per universe?

Yes. Radix will start with a shard space of 2^64 or 18.4 quintillion shards, which will be able to support every device in the world, and then some more.

### How does Radix avoids the [CAP theorem](https://en.wikipedia.org/wiki/CAP_theorem) limit?

Tempo relies heavily on eventual consistency to achieve a total ordering of events. Radix avoids the CAP Theorem limit that DAGs and Blockchains suffer from by allowing transactions to be both asynchronous and concurrent. This allows it to scale out limitlessly. The Radix shard space is key to this scalability - 18.4 quintillion shards that nodes can maintain concurrently,  while the Tempo consensus makes sure double spends don’t occur across these shards without being detected and rejected.

### How scalable will Radix DEX be?

We have yet to see how decentralized exchanges will behave with high traffic and whether they may be able to function as expected or even outperform centralized exchanges. A key part of product development is to ship the MVP, iterate and improve as we get live feedback from the users. So the DEX is projected to improve and scale as we understand how it behaves in a live environment.

