# Glossary

## Account

An **Account** represents all the data stored for a user on the ledger. This includes tokens, but also arbitrary data, as well as more advanced types of transactions in the future such as multi-sig and Scrypto smart contracts.

## Address

An **Address** lives in a **Shard** and is the start and end point for any **Atom** in the Radix Universe. It's also a reference to an **Account** and allows a user to receive tokens and/or data from other users. A Radix address is generated from a public key and a **Universe** checksum.

{% hint style="info" %}
Note: The defined **Universe** affects the generated **Address**.
{% endhint %}

## Atoms

An Atom is a collection of Particles whose properties are defined by the Quarks they are composed of. The Atom is actually nothing more than a container which contains a collection of Particles to be atomically committed to the ledger, and also acts as a receiver and carrier of consensus information relating to that Atom to be used in the event of a conflict.

Particles are instructions about a state change with certain properties, defined by its Quarks. In order to solve the state-sharding problem, the components of the state change need to be discrete elements that can hold independent state and tracked within the ledger, which the Particles assist in allowing. When a Particle is created, it is in the UP state, when it is consumed, or superseded, it is in the DOWN state. The concept is similar in its most basic operation to the Bitcoin UTXO.

To create a tokenized asset on the Radix ledger, first create an Atom with a TokenClassParticle in it that describes your token's properties. The TokenClassParticle itself has the OwnableQuark \(i.e., it has an owner with special privileges\), AccountableQuark \(the particle is stored in an account / at an address\) and NonFungibleQuark \(the particle is unique, and only one of it can exist\). Put this TokenClassParticle in an Atom with an UP spin, add a TimestampParticle \(to provide a wall clock time of submission\), sign the Atom, submit it to the ledger. Assuming a multi-issuance supply model has been chosen for the token, the owner can now mint tokens by creating OwnedTokensParticle \(which itself has the Ownable, Accountable and Fungible quarks\) in the UP state.

Another example is being able to pack many token transfers \(multi-sender, multi-receiver events\) into a single Atom by including many OwnedTokensParticles which are then in the DOWN state for senders, and UP state for receivers when. The entire collection of transfers is committed atomically and is much more efficient across storage, communication and IO/CPU costs.

Additionally, the Particles become very powerful over the long term road map, first by allowing 3rd party Particles to be composed from the initial Quark set easily, and ultimately resulting in definable 3rd party state machines & schemas to allow 3rd party Quarks to be created with defined constraints, from which very exotic Particles can be created from the composition of those.

For more information, see the [Tempo white paper](../whitepapers/tempo.md#radix-tempo).

## Commitments

To assist with total order determination of events, nodes declare to the network a periodic commitment of all events they have seen. A commitment is a Merkle hash.

## Decentralized Applications

Decentralized applications \(dApps\) are applications that run on a P2P network of computers rather than a single computer. dApps have existed since the advent of P2P networks. They are a type of software program designed to exist on the Internet in a way that is not controlled by any single entity.

## Distributed Ledger Technology \(DLT\)

A distributed ledger \(also called a shared ledger, or referred to as distributed ledger technology\) is a consensus of replicated, shared, and synchronized digital data geographically spread across multiple sites, countries, or institutions. There is no central administrator or centralised data storage.

## DSON

DSON is a binary in-house serialisation format which is tuned specifically for Radix. At very high throughput, serialisation / deserialisation becomes a major bottleneck, and JSON / GSON were not performant enough. Optimisation to squeeze as much out of JSON/GSON made the code in those sections very complex and messy. A simpler solution was just to bake our own with much higher performance, and that could be trans-coded to JSON for all our RPC/Restful needs.

## Gossip protocol

A gossip protocol is a highly effective, well-established way of getting information to be quickly spread through a network. In essence, any Node that receives a new piece of information tells two \(or more\) neighboring nodes about it. They then tell two or more each \(2\*2\) and so on. This follows an exponential pattern until all Nodes in the network have been told.

This is how Radix ensures any new transactions are quickly shared with all relevant nodes in a highly reliable, fast and fault-tolerant way.

## Identity

An **Identity** represents a private key which can sign **Atoms** and read encrypted data. This private key can be stored in the application, or in the future, it might live elsewhere such as the users wallet application or hardware wallet.

{% hint style="info" %}
The only type of **Identity** currently available is the _**Simple Identity,**_ and it has a private key stored in a local file.
{% endhint %}

## Logical Clock

Within Tempo, all Nodes have a local logical clock; an ever-increasing integer value representing the number of events witnessed by that node. Nodes increment their local logical clock when witnessing an event which has not been seen previously. Upon storing an event the Node also stores its current logical clock value with it. This record can then be used to help validate the temporal order of past events if required.

## Nodes

A **Node** provides general computing and networking resources to the network. Nodes are responsible for validating events and transactions, relaying messages, resolving conflicts and executing scripts on the network. They also maintain a subset of the shard space and get fees in proportion to their work.

## Partial Ordering of Events

Partial ordering of events is often mentioned but not explained. Example: "If you have three events `{A, B, C}`, then they are totally ordered if they always have to happen in the order `A > B > C`. 

However, if `A` must happen before `C`, but you don't care when `B` happens, then they are partially ordered. In this case we would say that the sequences `A > B > C`, `A > C > B`, and `B > A > C` all satisfy the partial ordering.

## Payload Atoms

Payload Atom are transactional messages, sent to one or more parties, like an email or an instant message.

For more information, see the [Tempo white paper](../whitepapers/tempo.md#radix-tempo).

## Shard

A public Radix network \(Universe\) is segmented into a very large shard space \(currently $$2^{64}$$ shards\). The start and end point for any Atom in the Radix Universe is an address, which is formed of a public key and a Universe checksum. The shard number of an address is deterministically calculated by taking a modulo of a public key over the total shard space to derive the shard index. This makes it trivial to for anyone to correctly calculate the shard a public key lives on.

For more information, check the Public Node Incentives [white paper](../whitepapers/public-node-incentives.md#shard-space).

## Temporal Proof

A temporal proof is a cryptographically secure record of ordered events.

Before an event can be presented to the entire network for global acceptance, an initial validation of the event is performed by a subset of nodes which, if successful, results in: A Temporal Proof being constructed and associated with the Atom, and a network-wide broadcast of the Atom and its Temporal Proof.

For more information, see the [Tempo white paper](../whitepapers/tempo.md#radix-tempo).

## Transaction Finality

Transactional finality refers to the instant that a transaction is deemed immutable, irrevocable and thus completed.

On Radix, transaction finality is the time it takes for a new transaction to have gossiped to the furthermost side of the relevant Shard from the starting Node, and then back again. As long as no conflicts are found, this transaction is then deemed to have reached finality.

## Transfer Atoms

Transfer Atoms are used to transfer the ownership of an item, such as currency, to another party.

For more information see the [Tempo white paper](../whitepapers/tempo.md#radix-tempo).

## Universe

An instance of the Radix Tempo ledger is called a Universe.

## Vector Clock

Within Tempo, all nodes have a local logical clock \(see [Logical Clocks](glossary.md#logical-clock)\)

These Logical Clocks can then be made into Vector Clocks, where the Logical Clock of many Nodes are collected in relation to the same event, and then used to create a comparison of order between Nodes in order to determine which event proceeded which.

Note: on their own, Vector Clocks only create a partial ordering - Node Commitments are also necessary to complete the ordering of events.

See the Tempo white paper for [more details](../whitepapers/tempo.md#commitments).

