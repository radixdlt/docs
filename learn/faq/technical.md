# Technical

## FAQ

This document is a compilation of questions that have been asked by the Radix community members across different channels.

### What are logical clocks?

Within Tempo, all Nodes have a local logical clock; an ever-increasing integer value representing the number of events witnessed by that node. Nodes increment their local logical clock when witnessing an event which has not been seen previously. Upon storing an event the Node also stores its current logical clock value with it. This record can then be used to help validate the temporal order of past events if required.

### Do logical clocks increment for only consumables or all events?

Logical clocks are incremented for all events, which is either protocol events \(atoms\) or protocol messages \(fault detection\)

### What are vector clocks?

Within Tempo, all nodes have a local logical clock \(see [Logical Clocks](https://www.radixdlt.com/faq/logical-clock)\)

These Logical Clocks can then be made into Vector Clocks, where the Logical Clock of many Nodes are collected in relation to the same event, and then used to create a comparison of order between Nodes in order to determine which event proceeded which.

Note: on their own, Vector Clocks only create a partial ordering - Node Commitments are also necessary to complete the ordering of events.

See the Tempo White Paper for more details: [https://papers.radixdlt.com/tempo/\#commitments](https://papers.radixdlt.com/tempo/#commitments)

### What are merkle trees?

Radix uses logical clocks to provide a simple and reliable method for nodes to record and recall events in the order they have witnessed them.  While these logical clocks are simple and reliable, they do not provide any security against nodes tampering with the logical clock values that it has assigned to events.

To ensure the assigned logical clock values are tamper-proof, nodes produce “Commitments” which are an ordered, cryptographically secure, compact representations of the events witnessed by a node.

### What are commitments?

To assist with total order determination of events, nodes declare to the network a periodic commitment of all events they have seen. A commitment is a merkle hash.

### How often does a node present commitments to the network?

Commitments are produced either when a node takes part in Temporal Provisioning for an event, or at will over an arbitrary interval. Any transaction a node sees will be part of its commitment in the temporal proof it signs. If a node is not eligible to sign the temporal proof, it will include all transactions it has seen since. It's last commitment in the next temporal it is eligible to sign. Unless the network is very large, most nodes will sign all temporal proofs for all transactions they see.

### What if the node is requested to send the commitment for a transaction that is not included in a commitment yet? Will that never happen or will it force the node to create a commitment on the fly?

Nodes don't  get asked for specific transactions. They get asked to prove prior commitments. If a node hasn't seen a transaction it won't be in it's commitment, but that's doesn't prevent it proving it's honest.

### What are temporal proofs?

A temporal proof is a cryptographically secure record of ordered events.  
Before an event can be presented to the entire network for global acceptance, an initial validation of the event is performed by a subset of nodes which, if successful, results in: A Temporal Proof being constructed and associated with the Atom, and a network-wide broadcast of the Atom and its Temporal Proof. ****For more information, see: [https://papers.radixdlt.com/tempo/\#radix-tempo](https://papers.radixdlt.com/tempo/#radix-tempo) 

### What is Byzantine Fault Tolerance?

**Byzantine fault tolerance** \(**BFT**\) is the dependability of a [fault-tolerant computer system](https://en.wikipedia.org/wiki/Fault-tolerant_computer_system), particularly [distributed computing](https://en.wikipedia.org/wiki/Distributed_computing) systems, where components may fail and there is imperfect information on whether a component has failed. In a "Byzantine failure", a component such as a [server](https://en.wikipedia.org/wiki/Server_%28computing%29) can inconsistently appear both failed and functioning to failure-detection systems, presenting different symptoms to different observers.

### What is Byzantine Fault Detection?

Distributed systems are subject to a variety of faults and attacks. A faulty/malicious node may exhibit arbitrary behavior. In particular, a faulty node may corrupt its local state and send arbitrary messages, including specific messages aimed at subverting the system. Many security attacks, such as censorship, freeloading, misrouting, and data corruption, can be modeled as Byzantine faults. Systems can be protected with Byzantine fault tolerance \(BFT\) techniques, which mask a bounded number of Byzantine faults, e.g. using state machine replication. BFT is a very powerful technique, but it has its costs. In a practical system that needs to tolerate up to f concurrent Byzantine faults, BFT cannot be implemented with less than 3f+1 replicas. Moreover, BFT scales poorly to large replica groups; as more servers are added, the throughput of the system may actually decrease.   
  
Byzantine Fault Detection is an alternative approach that aims at detecting rather than masking faulty behavior. With this approach, the system does not make any attempt to hide the symptoms of Byzantine faults. Rather, each node is equipped with a detector that monitors other nodes for signs of faulty behavior. If the detector determines that some node has become faulty, it notifies the application software, which can then take appropriate action. For example, nodes can cease to communicate with the faulty node; once all correct nodes have followed suit, the faulty node is isolated and the fault is contained. Lies \(or faults\) in Radix can be detected using merkle trees of commitments. 

[Read more about the case for Byzantine Fault Detection in this whitepaper.](https://pdfs.semanticscholar.org/5c6d/70758691d8d15137794acadbc7742a801e2b.pdf)

Radix uses both BFT to defend against network splits/attacks and BFD to defend against malicious behavior like double spends, transaction suppressing, etc.

{% hint style="info" %}
[Have questions about Byzantine Fault Detection? Ask them on our developer forum](https://forum.radixdlt.com/t/what-is-byzantine-fault-detection/53)
{% endhint %}

### What is mass?

We label events within the network \(such as messages or transactions\) as ‘[atoms’](https://www.radixdlt.com/faq/atoms). These atoms carry mass if they are transferring value, for example transactions or payloads with fees. This mass is calculated in a very simple manner, simply being the quantity \(e.g. the amount of Radix being transferred\) multiplied by the amount of time it has been static for. For example, if I transferred 10 Radix to Bob which I had held for 10 days then the mass of the atom being sent would be 100. If I had 5 Radix but had held for 20 days then the mass would similarly be 100.

The first node in the[ temporal proof](https://www.radixdlt.com/faq/temporal-proof) for a valid transaction receives this mass. Nodes then build up mass across the multiple transactions they validate. The record of this mass is stored by nodes serving the same shards as said recipient node, as they store temporal proofs from completed transactions \(known as finality\). It is important to remember that transactions don’t just have a single gossip path through the network but rather have multiple routes due to the nature of our gossip protocol, which sees nodes ‘talk’ to all of their neighbouring nodes. This means that transactions are seen by a large majority of nodes.

This mass allows us to resolve conflicts which cannot be solved by simple resolution by letting us use mass as the determining factor. We add up the mass of all nodes involved in the temporal proof for the conflicting transactions and the atom with the most total mass associated with it is accepted. The losing transaction is meanwhile discarded.

Expanding upon this logically, in the event of a network split whichever network has the greatest mass is the mainnet. The mainnet then absorbs all non-conflicting transactions \(i.e. anything that doesn’t constitute a double spend\) into it. Conflicting transactions are, again, discarded. This works because although both transactions will have reached finality on their respective networks, the transaction on the mainnet will possess the mass of this large majority of nodes involved in validating it, meaning it will sum greater than the subnet.

### Why do we need mass?

As discussed above, we use mass to defend against complex network attacks. [You can read more about it in this blog post](https://www.radixdlt.com/post/complex-conflict-resolution).

**T**he principles of mass is very simple, but in practice there are many things to consider...for example, in a large network, how do you collect enough information to determine all votes, without all nodes having to ask all other nodes \(which gets SUPER exponential very quickly\).

Dan assumed there would be a clever solution out there somewhere he could use, and so he read endless amounts of prior work and they all go exponent after a particular network size...so it was something else he had to invent.

The algorithm he came up with costs on average 1.05 messages per node and 12kb of data to determine the vote of a 10k node network to a suitable accuracy that we require.

Furthermore it's consensus friendly so that all nodes will output the same result \(with some margin for error\) based on the same input

To put that in perspective, that's like 10k people voting on something and you have to ask just one person what they know of how a handful of other people voted and you can arrive at an accurate and agreeable prediction for all 10k people.

It scales well too, for a 100k node network it's still around 1.05 messages per node average and the data cost per node is about 27kb  


### **How is Mass calculated?**

Mass = XRD \* \(consumable \(output\) universe planck - consumer \(input\) universe planck\). A universe planck period being an hour on mainnet because seconds are too granular to use and as our ledger wall clock time is only approximate, we need to have an adequate buffer to absorb drift, etc.  


### Will other user generated tokens have mass?

Probably yes. More details will follow in the upcoming security whitepaper

### Will Radix be interopable with other blockchains?

No. Radix has a

### Will Radix have smart contracts?

Yes, Radix is developing a unique smart contracting language that can be learnt by even beginners. More details will follow as we progress through out [Roadmap milestones](https://radixdlt.com/roadmap).

### Is Radix Quantum Computer Resistant?

Like most systems the Radix ledger is not Quantum Computer resistant. Currently we are looking at quantum resistance to determine either;

* What cryptographic technology we can implement now
* How can we migrate to quantum resistant cryptographic algorithms safely in the future

Ideally we like option 1, but a lot of the algorithms available are stateful, or produce VERY large signatures which is not ideal. Unless we can get a quick win developing a version that is stateless, we’ll probably end up with option b for now as rolling our own crypto which is not time tested is dangerous. This helps us focus on key features.

### What are some benefits of Temporal Proofs?

Temporal proofs help with Mass delivery, propagation information \(gossip verification\), logical clock information, commitment information \(for fault detection\), RTP \(time sync\) information. They do do all of that in 100 bytes per vertex.  


### **Is data encrypted where the node hosting the copies of data won’t be able to tell what’s in it?**

It's my understanding that transactions are signed with the private key of the entity owning the radix, just as in bitcoin.  I have no idea about the message type atom.  
When it comes to data/messages/payload, the sender decides what to encrypt and what keys to use for the encryption. For a Twitter-like dapp the data would be in clear text since it's supposed to be public. For a private message the data would typically be duplicated and encrypted with the sender's and the receiver's public keys, so that both sender and receiver can decrypt it later. For private data storage, the data would be encrypted only using the sender's public key since no one else is supposed to read it. A node can only read the data that is sent in clear text.

### **Which hashing algorithm is used in Tempo?**

Yes, we use SHA126 to hash the history of work performed b the node. It’s a chained merkle tree also called as commitments.

