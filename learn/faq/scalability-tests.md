# Scalability Tests

## Why did you use the Bitcoin blockchain to perform this test?

We’ve chosen the bitcoin ledger transaction history as it is an easily verifiable, familiar data set, containing [460 million addresses](https://blog.chainalysis.com/reports/bitcoin-addresses) and over 400 million transactions. Moreover, bitcoin and radix share the same fundamental building block - UTXO.

## **What was the full process behind the scalability?**

We’ve prepared a blog post explaining in detail each and every decision along the way, starting from the concept up until the test itself. In case you are interested, please go to: [https://www.radixdlt.com/post/test-method-part1/](https://www.radixdlt.com/post/test-method-part1/) \(Non Technical\)  
[https://www.radixdlt.com/post/test-method-part2/](https://www.radixdlt.com/post/test-method-part2/) \(Technical\)

## **Radix DLT is not a blockchain, so how are the transactions stored?**

We are translating BTC transactions to our Atom model - 1 bitcoin transaction is equal to 1 atom being sent.  
****

## **I’ve recently made a transaction using bitcoin. How can I check if it’s been processed during the test?**

We will be repeating this test periodically. Though the test itself will probably be completed much earlier, you will be able to validate your address for an hour. Head to [https://radixdlt.com/test](https://radixdlt.com/1mtps) to see live tests and to see prior test results as we progress.

## **How many nodes have been used for the purposes of this test? Are these just servers in a room?**

We use 1,000GCP \(Google Cloud\) nodes evenly spread among at least 11 datacenters across the globe to create a fair test environment. The nodes we use are n1-standard-8. More info available at [https://cloud.google.com/compute/pricing\#predefined](https://cloud.google.com/compute/pricing#predefined).

## **Do you use sharding?**

Yes, the Radix ledger and the test data is fully state sharded \(2^64 shards, which we split up into 100+ shard groups\)

## **Will this test include double-spends or any kind of attacks?**

The bitcoin ledger transaction history does not include those, so no, not for this test.

## **Will the consensus and validations signature mechanism be on?**

Yes, this test is intended to recreate real-world conditions

## **Do you intend to test datasets that will include double spends?**

Yes, future tests will include those

## **I don’t believe you, I want to run the test on my own. Can I?**

Sure, here’s the GitHub link - [https://github.com/radixdlt/mtps](https://github.com/radixdlt/mtps) for running it yourself.

## **Where do these tests deviate from a main-net radix deployment?**

We're bypassing the WebSocket API normally used for submitting atoms, and instead, pre-load them into memory before the test and inject them directly into the submission queue. Redundancy is around 4.2 nodes per transaction, which is low to save cost.

1. Each node receives all atoms relevant to its shards directly from the atom pump. Nodes are still trying to synchronize as normal, but only a small fraction of atoms get sent between them over the network. 
2. To emulate BTC being mined \(coinbase inputs\), we allow creating tokens out of thin air 
3. Since the Radix ledger doesn't support the full capabilities of BTC scripts, any advanced transactions such as timelocks have been replaced with sending the funds to a randomly-generated intermediate address 
4. The test is run within the Google Cloud Platform data-centers, which means that while distributed around the world,  packet loss is lower than in the real world

