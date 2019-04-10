# Security

## Radix Security Model Basics

During the final leg of our alpha launch event, Dan gives a quick primer on the Radix Security Model. Anybody who wants to learn more Radix security should start by watching the following video:

{% embed url="https://www.youtube.com/watch?v=7xc9TNKMNLY&t=2201s" %}

## Preventing Double Spend Attacks

_Radix uses logical clocks for generating a causal ordering of events to detect and prevent double spends._

The first event to gain a temporal proof will become the valid event, the second event will be automatically discarded by the nodes as soon as an earlier temporal proof is presented. As this will happen within the time it takes for both transactions to be gossiped to all Nodes in a shard, the certainty of a transaction can be generally reached within 5 seconds or less.  


## Preventing Sybil Attacks

{% hint style="info" %}
[Blog post on what are Sybil attacks](https://www.radixdlt.com/post/what-is-a-sybil-attack)
{% endhint %}

When thinking about Sybil in Radix, you need to consider where Sybil would give you a distinct advantage.

1\) Eclipse attacks   
2\) Consensus attacks

Neither of these is easy or cheap to pull off...

{% hint style="info" %}
[Blog post on Eclipse attacks](https://www.radixdlt.com/post/what-is-an-eclipse-attack)
{% endhint %}

### Sybil protection for Eclipse Attacks

All nodes have a NID \(Node Identifier\) which is produced by creating a POW hash of sufficient difficulty using its public key and some ledger meta-data. We won't detail the algorithm used to decide which ledger data is used for the POW here, but it results in ASIC resistance. 

If I want to eclipse a node \(or set of nodes\), then because NIDs and NID distance drive connections and gossip, I need to calculate my NID to be as close to the NID of the node I wish to attack as possible. 

If I don't, I can't have any strong guarantees that I can eclipse them, or that my eclipse is temporary \(another new node joins that has a NID closer to my target than I do. Therefore the eclipse is broken, and the target node has access to the truth\). 

Generally for me to create a NID in an acceptable range of a target NID requires me to generate as many POWs for my public key as there are nodes in the network. In a healthy network of 10,000 nodes \(similar to BTC and ETH\), that's 10,000 hours of work for a NID that is in range. 

The target node probably holds more than 1 TCP connection, the default is 8, but node operators can configure however many they would like. To entirely eclipse that node via its TCP connections, I would need 8 NIDs in range, so about 80,000 hours of work. 

Exaggerating the problem further is that I never actually know if I have eclipsed that node. Say I create 8 NIDs for my target, but only 6 of them can connect. Does that mean that the node only has 6 connections in total and is eclipsed? Or are 2 of its connections already occupied by other NIDs that I don't control. 

My only option here is to continue trying to connect to it, in the hope that the nodes I don't control disconnect from it, and I can connect to it before a connection to another node is established. Maybe all 8 of my nodes connect, but I still don't know the status of the eclipse, maybe that node is configured to hold 9+ TCP connections. 

Finally, nodes use TCP for sync and UDP for gossip. UDP doesn't have any connection limit, and I as an attacker can not prevent any node in the network talking to my target over UDP, nor can I prevent my target from talking to any other node over UDP. 

My target can still receive truthful statements, from anywhere in the network, and there is nothing I can do about that as an attacker. Even if I have managed to eclipse them from a TCP/Sync perspective, they will likely receive truthful information over UDP that conflicts with whatever it is I am telling it. 

Full eclipse attacks in Radix are pretty much impossible .

### Sybil Protection for Consensus Attacks

POW in Bitcoin mining is used to ensure that only one vote can be made every 10 mins \(on average\). It's a very simple Sybil protection mechanism that means no matter how many machines I have, or miners, or identities, the probability is that I can only vote \(create a block\) once every 10 mins.

The issue is that POW vote Sybil is not very scalable or efficient.

In Radix, Mass is used as our "POW" vote Sybil mechanism, and mass is a product of an Atoms value \(which can be monetary, data, or some other metric\) and time. The longer something is static, the more mass it achieves.

This mass is then distributed to the nodes in the network depending on their position in the Temporal Proof for that Atom.

Each Atom takes a different route than another Atom depending on its origin point, which makes predicting where and who Atoms are going to come from to you next totally unpredictable.

When I receive an Atom, and validate it, I "stamp" the Temporal Proof with my approval, essentially a vote, which is weighted by the amount of mass that NID controls.

In order for me as an attacker to control consensus and double spend, I need to control &gt; 51% of all the mass in the Radix universe.

Now, a 51% attack in Bitcoin is expensive but possible, just buy lots of ASIC hardware, setup and go.

Not so in Radix. To control the universe you need mass. Mass is produced by mass carrying Atoms \(transactions mainly\). The longer some funds have been in an account, the more mass they produce.

In order to have enough mass to mount an attack, I as an attacker would have to be the initial validator for &gt; 50% of all Atoms in the universe.

I can produce my own Atoms, which would require a HUGE amount of funds at my disposal, which I then need to manage to extract the maximum amount of mass from them \(which generally means I would have to hold 50%+ of ALL XRD in existence at all times\), not to mention the fees.

A better option is probably to be the initial validator for as many of everyone else's Atoms as possible.

To do that I need to create an equal amount of nodes in the network as there were before I started. If there were 10,000 nodes before I started, I need to create 10,000 nodes of my own.

I need to generate 10,000 NIDs, distributed as evenly as possible throughout the NID space and pay the 10,000 hours cost of doing so.

Finally, I need my 10,000 to be connected to as many transaction producing clients as possible. If a client holds 8 connections to the network, I need at LEAST 4 of those in order to catch my 50%+ mass, for EVERY connected client.

As with eclipsing though, even if I do all that, there's a final nail that kills me dead.

50% of the mass from now isn't enough, as all the nodes that were in the network before me already have mass. I actually need 50%+ of all the mass ever generated in order to be sure my 51% attack will actually be near successful, which in turn means that I need to control much more than 10,000 nodes to do so in a reasonable amount of time.

{% hint style="info" %}
[Have questions? Join us on the developer forum to discuss how Radix prevents Sybil attacks](https://forum.radixdlt.com/t/preventing-sybil-attacks/45)
{% endhint %}

## How can we measure/confirm that a shard has "enough" nodes to maintain a healthy level of security?

The more nodes that have been in a shard, the more secure a shard history is due to the number of redundant copies of the shard information being maintained. 

It is also possible for nodes to leave a shard, come back later and re-affirm history. This makes any attack vector not just all the nodes in a shard now, but all the nodes that have ever witnessed the event you are trying to change.

There is also a high degree of incentive for nodes to maintain under-served shards as the more shards low node count shards they maintain \(serve\) the higher the probability of getting fees.

For shard selection incentives, see: [https://papers.radixdlt.com/incentives/\#shards](https://papers.radixdlt.com/incentives/#shards)

{% hint style="info" %}
[Discuss this topic on the developer forum](https://forum.radixdlt.com/t/how-can-we-measure-confirm-that-a-shard-has-enough-nodes-to-maintain-a-healthy-level-of-security/46)
{% endhint %}

## Can a bad actor take control of a shard?

With some difficulty - as the Radix universe is split into 18.4 quintillion shards, and your shard address is deterministic on your wallet address, you would need to continually cycle PGP key generation to get matching shard address keys. In addition to this there are several other sybil and spam attack prevention mechanisms to prevent even high numbers of malicious actors on a shard being able to reliably control or isolate honest nodes in a shard.

These mechanisms will be covered in more detail in the forthcoming v2 Tempo White Paper.

{% hint style="info" %}
[Discuss why the probability of someone taking over the shard is almost impossible.](https://forum.radixdlt.com/t/can-a-bad-actor-take-control-of-a-shard/47)
{% endhint %}

## How does Radix prevent spam transactions?

The cost of sending transactions across the Radix network is currently set at $0.01, while the cost of validating a transaction is almost negligible in terms of computer resources. 

This makes the attack highly asymmetric in favor of Nodes, meaning that an attacker would have to spend a very high amount in fees to create any kind of stress on the network. If an attacker spams the network, nodes are immediately incentivized to validate them and collect fees for work done, in extreme cases, attracting more nodes to the shard being attacked due to the strong, positive economic incentives.

## How does Radix prevent nodes from suppressing transaction announcements?

Nodes in the Radix network periodically commit the Merkle hash of all previously observed events. Suppressed transactions are easy to detect with the help of these periodic commitments. For example: If a Node\(A\) sends a message to Node\(B\) at logical clock 10 asking for Atoms that B might have that A does not and B responds that it has nothing then both nodes include the hash of the messages in their commitments. 

If B was withholding an Atom to present later to try and double spend, and presents it, and Node\(A\) sees it at logical clock 20 for example, Node\(A\) will have in one of its commitments that it asked Node\(B\) for Atoms it doesn't have at logical clock 10 and B said it had nothing. Node\(B\) is therefore exposed, with proof that it lied which can be broadcasted to the network.

{% hint style="info" %}
[Discuss this topic on the developer forum](https://forum.radixdlt.com/t/how-does-radix-prevent-nodes-from-suppressing-transaction-announcements/49)
{% endhint %}

## How does Radix prevent 99% malicious nodes in a shard?

Building on the ideas of Leslie Lamport in his seminal 1982 paper  “[The Byzantine Generals Problem](https://people.eecs.berkeley.edu/~luca/cs174/byzantine.pdf)” \(also recently [discussed by Vitalik Buterin on his website](https://vitalik.ca/general/2018/08/07/99_fault_tolerant.html)\), Radix uses a consensus algorithm that can withstand up to a theoretical 99% dishonest nodes, while still being able to determine the truthful events from the malicious ones.

[Our sharding article looked at how the shards](https://www.radixdlt.com/post/sharding-in-radix) and data architecture of Radix allows for linear scalability. While this is important, scalability is useless if we cannot reach[ consensus](https://www.radixdlt.com/post/why-consensus-algorithms-are-like-military-generals). 

The permissionless consensus system employed by Radix applies to every transaction at the shard level. Radix uses logical clocks to order and validate transactions, as well as to determine the earliest transaction in the event of two \(or more\) conflicting transactions, with the conflicting transaction\(s\) ordered and discarded.

We call this process Asynchronous Byzantine Fault Detection - a review of the basics and why Fault Detection \(when properly implemented\) is so effective, please see this excellent paper by Haeberlen et Al - [The Case for Byzantine Fault Detection](https://pdfs.semanticscholar.org/5c6d/70758691d8d15137794acadbc7742a801e2b.pdf)

This relies on the network always being able to identify the difference between a main net and a sub net - the details of how we achieve this will be the subject of a later blog post. This blog post will simply give an overview of how we achieve event ordering on a shard, assuming no malicious subnets.

### **What are logical clocks?**

In a Radix Universe, an “event” is a general term for something that the ledger has to do at the request of a user. This could be a transaction, it could be a message, or it could be the updating of a state in a Smart Contract. A Node is a computer running part or all of the Radix universe - we will discuss these components in more depth in another article.

[Logical clocks](https://en.wikipedia.org/wiki/Logical_clock) are essentially a counter measuring events seen by nodes; every time a node sees an unseen and valid event it adds ‘1’ to its logical clock counter as follows:

1. Node receives a new event
2. Node checks its own database to check if it has seen it before, as well as doing basic cryptographic validity checks
3. If it is a new event for the Node, it increments its logical clock by 1 \(e.g. from 0 to 1 if it is the first new transaction it has ever seen\) and associates that value with the event in it’s own database
4. It then stamps the event with that logical clock value, as well as its own signature \(so that it cannot be forged by other nodes\) and then passes that event on to other nodes in the network
5. As the event moves through the network it collects the signatures and Logical Clock time stamps of all the Nodes that it touches until a threshold \(depending on network size\)

To jump into this a bit more, see this explainer video by Dan: 

{% embed url="https://www.youtube.com/watch?v=wfsZuN6NaJo" caption="Using Lamport Time Stamps \(logical clocks\) to create basic order within the Radix network." %}

### **Determining relative order**

Because [the way the ledger is constructed](https://www.radixdlt.com/post/sharding-in-radix), a spend from an address can NEVER be from two different shards - a wallet address determines the shard that wallet may spend from. This means that all related events \(such as a double spend\) MUST always be on the same shard.

Nodes are constantly gossiping these events to each other. This gossiping of logical clock values is used in conjunction with a node’s own logical clock values to allow nodes to adjust the absolute order of transactions and slot in new events into the right place.

In most scenarios, no further action is required by the Node. A Node hears about a new event, adds that event to their database with their current logical clock value, and gossips that out. If they hear no conflicting information back, then the transaction may be considered to be truthful. It is an incredibly efficient, passive consensus mechanism that only requires the comparison of two events in the case that there are two events that conflict.

A simple example of a conflict would be two spends of the same consumable \(e.g. a double spend\). In 95% of cases, this is simply a matter of comparing the collected logical clock values from all the nodes both transactions touched, and the transactions that came before and after that specific transaction.

For example, event X and Y conflict. If a node knows transaction X took place before transaction Z, but does not know when Y occurred. It simply has to find a node that saw both Y and Z. If Y is found to have occurred after Z, then Y may be discarded as the later transaction and the invalid second spend.

If Z does not exist, or Y occurred before Z, then the second stages of Radix consensus are engaged. This second stage is called Mass, and will be the subject of future blog posts, but a brief introduction can be found at the end of our Alpha launch video here:  

{% embed url="https://www.youtube.com/watch?time\_continue=2201&v=7xc9TNKMNLY" %}

### **Quick Sync**

This system also allows new nodes to ascertain the correct order quickly. Imagine Node A has a logical clock value of 10 and the newly joined Node B has seen no events. Node B would ask Node A for the values from its ledger and therefore see all of the events Node A has in the order they occurred. It would then add that data to its own ledger. This can be cross checked against multiple other Nodes, allowing Node B to quickly build a complete relative ordering of past events before it can start participating in the new event consensus.

### **Keeping nodes honest**

Although this allows us to establish relative order, it does not prevent nodes from lying about when they saw a transaction. Each logical clock is locally owned and the node could falsify the timing or the order of events.

To solve this, actions are associated with the node responsible for creating it to prove the node is faulty in the event of a failure \(which provides [aBFD](https://pdfs.semanticscholar.org/5c6d/70758691d8d15137794acadbc7742a801e2b.pdf)\). Radix uses what are known as Commitments to audit the logical clock output of nodes, ensure accountability and prevent tampering. A Commitment is a unique identifier created by five bits of information:

1. The node’s logical clock value at time of Commitment
2. The ID of the node submitting the Commitment
3. The timestamp for that node at time of Commitment
4. A signature from the node to prove it produced the Commitment
5. A Merkle hash

The initial four are fairly self-evident but the [Merkle hash](https://en.wikipedia.org/wiki/Merkle_tree) may require explanation. Each protocol event will have a unique reference known as a hash \(a long string of text and numbers\). We essentially add a number of these hashes together to give us a unique Merkle hash.

We do this as it allows for nodes to check the work of the submitting node. Not all nodes will have seen the relevant events included but still need to know if the Commitment is valid. To make sure the node isn’t lying, other nodes need a means to examine the data. The Merkle Hash is what allows them to accomplish this.

For example, assume a node is reviewing a Commitment from another node which has four events in it, but the reviewing node only knows about Event A. Before validating the Commitment, it first asks neighboring nodes for information on Events B, C and D. This will allow the node to know the order events were seen in \(by their logical clock values\).

The node can then check the Merkle Hash to make sure that the first node was telling the truth about when Event A occurred and that the four events occurred in that order. If these conditions aren’t met then the Merkle Hash will be different and as such so will the Commitment. This means the submitting node may have lied or tampered with the logical clocks of when it had seen events.

Furthermore, each new Commitment incorporates the information from the previous Commitment into it, which makes it harder to lie about past transactions. Ultimately, this simple technique means that all nodes MUST strictly increment their logical clocks by 1 for each new event as anything else is trivially detectable by the other nodes.

To dive into this a bit more, here is Dan again! 

{% embed url="https://youtu.be/jTYGOuS2xvg" %}

‍The final problem we face is that nodes could pick and choose when to submit Commitments or not produce Commitments at all. This would make it harder to know when they actually saw events. This is solved by forced periodic Commitments where nodes must share information. This means a node is unable to say it saw Event Z at a particular moment in time and then later lie, ensuring logical clocks increment strictly in order.

As mentioned above, this is a very passive consensus mechanism. It only needs to operate in the case of a conflict. In a network not under attack, this is less than 1 in 10,000 transactions \(non-malicious faults can still cause issues\). In 95% of cases, the above simple consensus mechanism resolves the conflict in a asynchronous byzantine fault detecting way.

The remaining 5% of non-malicious faults \(1 in 200,000\) or in the case of some forms of malicious attacks require a more exotic conflict resolution using Mass. This will be the subject of a later blog post.  
  
To learn more about the simple Radix consensus mechanism, please see our white paper here: [https://papers.radixdlt.com/tempo/latest/](https://papers.radixdlt.com/tempo/latest/)

{% hint style="info" %}
[Click here to continue this discussion on the developer forum](https://forum.radixdlt.com/t/achieving-consensus-with-99-dishonest-actors/40)
{% endhint %}

## Preventing network splits and double spends during splits

Our last article looked at[ simple conflict resolution](https://www.radixdlt.com/post/simple-consensus-in-radix), breaking down how we use components such as logical clocks and commitments to solve the majority of conflicts. However, there are a small number of conflicts which require a more exotic conflict resolution system. These conflicts bring forth a new component of Radix, ‘mass’.

### **Network splits and double spends**

We have previously analyzed the threat of[ network splits](https://www.radixdlt.com/post/what-are-network-splits) and[ double spends](https://www.radixdlt.com/post/what-is-a-double-spend-attack) to distributed networks. Being able to resist such attacks, which aren’t necessarily always deliberate or malicious, is a prerequisite of a functioning network. As a reminder:

**Network splits**: A network split can see entire geographic regions disconnected from the wider network. In the case of Bitcoin, a malicious actor would split from the main chain in a bid to outpace the block creation rate of the main network and thus overtake and become the main chain itself.

A network split in Radix would similarly involve the creation of two independently operating networks \(a mainnet and a subnet\). A user able to access both networks during this period could spend on the subnet and then subsequently also on the mainnet. When the two networks merged back together, the subnet would have earlier logical clock proofs than the mainnet and as such the mainnet transaction would be reversed and discarded. This is a subnet attack.

**Double spend**: A double spend is when the same output is spent twice, such as in the above. This can be malicious and done on purpose to steal from another user, or it can be by accident \(e.g. creating the same transaction twice by mistake\).

### **Why is our simple conflict resolution not enough?**

The previous article looked at simple conflict resolution works when all nodes are on the same network and therefore we can easily establish a causal history. However, when a portion of nodes are no longer on the same network - as in the case of a network split attack - this will no longer work because:

1. Nodes won’t have seen the same set of event activity, as the gossip that usually propagates all events would have been operating on the two different networks with no crossover. Instead of our previously structured gossip \(which gave us a causal history based off the predictable propagation of information across the network\) we find ourselves with two independent and conflicting sets of histories seen by different sets of nodes
2. The network would therefore have to expend significant resources to establish a single causal history of all of these ‘new’ transactions to work out the correct order, an effort which would be time-consuming. This delay would lead to further attack vectors for a malicious actor to exploit

As such, we need a new means to quickly and fairly resolve these more exotic conflicts.

### **What is Mass?**

We label events within the network \(such as messages or transactions\) as ‘[atoms’](https://www.radixdlt.com/faq/atoms). These atoms carry mass if they are transferring value, for example transactions or payloads with fees. This mass is calculated in a very simple manner, simply being the quantity \(e.g. the amount of Radix being transferred\) multiplied by the amount of time it has been static for. For example, if I transferred 10 Radix to Bob which I had held for 10 days then the mass of the atom being sent would be 100. If I had 5 Radix but had held for 20 days then the mass would similarly be 100.

The first node in the[ temporal proof](https://www.radixdlt.com/faq/temporal-proof) for a valid transaction receives this mass. Nodes then build up mass across the multiple transactions they validate. The record of this mass is stored by nodes serving the same shards as said recipient node, as they store temporal proofs from completed transactions \(known as finality\). It is important to remember that transactions don’t just have a single gossip path through the network but rather have multiple routes due to the nature of our gossip protocol, which sees nodes ‘talk’ to all of their neighboring nodes. This means that transactions are seen by a large majority of nodes.

This mass allows us to resolve conflicts which cannot be solved by simple resolution by letting us use mass as the determining factor. We add up the mass of all nodes involved in the temporal proof for the conflicting transactions and the atom with the most total mass associated with it is accepted. The losing transaction is meanwhile discarded.

Expanding upon this logically, in the event of a network split whichever network has the greatest mass is the mainnet. The mainnet then absorbs all non-conflicting transactions \(i.e. anything that doesn’t constitute a double spend\) into it. Conflicting transactions are, again, discarded. This works because although both transactions will have reached finality on their respective networks, the transaction on the mainnet will possess the mass of this large majority of nodes involved in validating it, meaning it will sum greater than the subnet.

### **Increasing the cost and difficulty of attacks**

For conflicting transactions on the subnet to ‘win’, the malicious actor would have to outrace the naturally much larger mass of the mainnet. This is both difficult and expensive and can be thought of as similar as to how Bitcoin or other Proof of Work networks resolve splits – whichever chain has the higher hashrate and thus the greater amount of mined blocks becomes the primary chain, with the other\(s\) discarded.

The obvious issue would be that someone could ‘fake’ mass on the subnet. However, this is impractical. Whereas hashrate can be increased by purchasing more mining equipment, mass in Radix is partially decided by the un-buyable commodity of time. Even if a malicious actor was to repeatedly spam their own nodes, the mass of each event would be low because the calculation of mass includes time held.

Equally, a malicious actor trying to acquire more Radix to make more transactions would find such a pursuit prohibitively expensive, even for the most well-resourced of actors, as they either have to lock up funds for extensive periods of time \(over which time the total mass of the network continues to increase naturally\) or they must get a very, very large number of funds and try and spam the network at a slow but continuous rate. This both eats away at the balance in fees, and the network continues to gather mass naturally, creating a very difficult to beat mass race condition.

Finally, whereas a Bitcoin network split would require miners to re-process the transactions lost from the smaller chain, Radix re-merges all non-conflicting transactions automatically. This makes it more suitable for use cases which may rely upon intermittent network connection \(e.g. payments on an airplane or in rural areas\).

As a result, the majority network should always have the most mass and thus will remain the mainnet, thwarting split attempts, whilst honest transactions remain unencumbered and intact. This is how Radix protects against the network split and double spend attack vectors. Our next article will look at how nodes operate on the network and how we avoid other potential threats such as Eclipse attacks.

{% hint style="info" %}
[Click here to continue this discussion on the developer forum](https://forum.radixdlt.com/t/preventing-double-spends-during-network-splits/52)
{% endhint %}

## 

