# Transactions

## FAQ

This document is a compilation of questions that have been asked by the Radix community members across different channels.

### Why does Radix use a UTXO model instead of a balance model?

Short answer - because if balances are stored, we cannot state shard without making the transactions non-atomic. A UTXO type system of consumables allow us to state shard with zero destination overhead, whereas a balance model ends up having a significant destination overhead, making sharding inefficient and only secure through a form of centralized orchestration.

Long answer - to answer this question we must first establish that sharding is necessary - generally speaking there are two constraints with a DLT:

1. Transaction throughput
2. Storage throughput

With a [directed acyclic graph](http://www.radixdlt.com/videos/radix-dag-scaling-issues-aims-of-radix/) \(DAG\), transaction throughput is higher than on a blockchain as the transport can be optimized  - you are dealing with transactions individually rather than as a block. However, those transactions still need to be validated, and at around 2,000 transactions per second on a single shard \(assuming standard servers\) you will see the performance of the full nodes on a network start to drop significantly. Pruning the ledger does nothing to reduce this load as this is a throughput constraint, not a storage constraint.

To scale beyond the 2,000 transactions per second mark, it is necessary to start splitting the throughput load; this requires [sharding the network](http://www.radixdlt.com/post/sharding-in-radix/), splitting the work amongst the nodes, rather than requiring all the nodes to do all the same work.

Now that you are splitting up the ledger into parts, you need a method of dealing with transactions between [shards](../glossary.md#shard).

Using balances, rather than UTXO can certainly reduce the storage requirements of the ledger on a given shard as you can prune the transaction history; however, one shards output is no longer another shards input without overhead. Since no TX history is being stored, just balances, the transactions between shards are not atomic, and this starts creating serious complexity. As a worked example:

There are four shards - A, B, C, D and I start with a balance of 50 on shard A and send 10 to wallet X on Shard B.

My balance is now recorded as 40, and at some unknown time in the future wallet X will have a balance of 10 \(let's disregard the mechanics of how wallet X becomes aware of the spend in shard A\).

However, I also send the same 10 to wallet Y on Shard C, and wallet Z on Shard D.

If I am only recording balances, and not UTXO; and only one of those transactions is valid, how do I void the updated balances of the wallet on the other two shards once the triple spend in Shard A is resolved?

The overhead I tried to avoid by making transactions non-atomic has simply moved to the double spend resolution process, as I now have substantial inter-shard communications to perform to correct the double spend as the output of one shard is not the input of another; we have no transaction history to construct a balance, just the balances themselves.  

Now there is a co-ordinator requirement again - some sort of central authority with a guaranteed, up to date view of all shards, to make sure that any given spend only updates 1 of the balances on any shard.

Without that, without transaction history, and without atomicity, double spending between shards becomes trivial.

### How long does it take for transaction settlement?

 Transactions on the Radix Network confirm and finalize in 10 seconds or less. Current test networks may experience some delays, as we test and break things. 

### What is the transaction fee?

$0.01 to start with - this is set arbitrarily high to prevent spam attacks. Fees will gradually decrease as paid operations on the network increases. 

### Are fees fixed?

Yes.

### Are transactions private and/or anonymous?

Radix is as private as bitcoin. It does not support complete anonymous transactions but will support pseudo-anonymous transactions in the medium term. The ledger is designed as such that in the future it will be very easy to create an anonymity layer on top of it. The team will not focus on it until the basic features are stable.

### Are all Tokens first-class citizens on the Radix platform?

Yes - essentially this means you can pay the transaction fee with the token you are sending, e.g., “Coffee Token” rather than Rads.

This is to make sure that anyone who creates tokens on our system is not forced to also make sure their customers and coin holders must also use Rads if they want to use the system. This was specifically designed into the Radix protocol to make sure all tokens are first class citizens.

This does, however, rely on:

1. The Radix DEX \(decentralized exchange\) being live \(white paper pending\) - part of the Radix protocol.

2. That the token you want to pay the fee with, and the Radix token, have liquidity as a traded pair on the DEX.

Once these two facts are live; it is then possible for anyone who pays the transaction fee with their own token as the transaction fee will be converted to Radix tokens on the DEX; allowing the node to confirm any token transaction and receive XRD instead, without the sender requiring to hold XRD to send their tokens.

However, if your Coffee Token is not popular enough to have a trading pair on the DEX, then you will still need to pay the transaction fee in Rads.

### How do I receive test radix tokens?

Download, install and take the Alphanet wallets for a test drive on your desktop or Android phone. [Download here.](https://radixdlt.com/wallet)

### If all transactions directed to a smart contract are stored on one shard \(the shard matching the address of the contract\), how can nodes serving this one smart contract scale beyond the expected computational power of any given computer on that shard range?

Yes, your upper bound for a smart contract is indeed the fastest nodes that serve the shard within which it lives, but that is the same for transactions too, a shard cannot process more transactions than the fastest machines which serve it \(which right now is about 3000 transactions per second for a commodity machine\). If a particular smart contract is very popular, then it is generating a lot of fee revenue for the nodes that serve it; so we expect to see those nodes contract their serving shard group around that particular shard as much as possible allowing them to free up CPU from serving other shards, and allowing them to process more in the smart contract shard.

### **What's the cost of running smart contracts on radix? Will there be a gas system?**

Kind of, but its called "Joules" and works a little differently than how Ethereum manages it. The stable and scalable aspect of Radix will make it a much more predictable unit cost.

