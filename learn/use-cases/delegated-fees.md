# Delegated fees

## Introduction <a id="docs-internal-guid-cb3509ab-7fff-c1f6-69d8-d466c386bafe"></a>

A “delegated fee” or “meta-transaction” enables a third party to pay for a transaction fee on behalf of a user. This article explains how to create distributed applications \(DApps\) on Radix using fee delegation, so users are not required to own XRD tokens to execute or interact with them.

## Network fees

The network transaction fees are necessary to pay for the network infrastructure, the nodes, and to avoid spam. However, they can be a big problem when building an application aiming to give service to millions of users. No business is expecting its users to go to the market and buy tokens, nor wants to give them away to accounts that may never use them.

One of the most common concerns when deciding whether to build applications on top of public DLTs is the need for users to own the network utility tokens \(on Radix, the XRD\) that are required to pay for the transaction fees. Users are helpless without having the underlying utility token, and this locks out a large portion of the population from adopting blockchain DApps.

## The Radix alternative

The Radix Engine supports specific features that can easily be used to solve this issue. As explained in the [Atom Model article](../architecture/atom-structure.md), atoms can be crafted in many ways, and they can contain many transactions inside them. Each atom can contain multiple [particles](../architecture/atom-structure.md#particles) and signatures, so that either they are all valid, or they are discarded.

When building on Radix, the typical architecture would be a server running the application or game, and a client that keeps the users' keys and operates either directly to the ledger or to the server.

To let users utilize the DApp without having XRD on their accounts, the application could simply create an extra service that receives incomplete atoms from clients, validates their contents and attach its own signature to pay for the fee.

## Implementation

In order to implement this solution, each time that a user interacts with the DApp, the service has to sign a specific UTXO, and the client needs to know this upfront to prepare the atom.

The steps are as follows:

1. The service starts by dividing funds into 10 different UTXO
2. A client authenticates with the server
3. The client requests a UTXO to pay for the fee through an API
4. The service selects an available UTXO, returns it to the client and flags it as unavailable.
5. The client generates a new atom with the service-provided XRD UTXO and signs it
6. The client sends the signed atom to the service
7. The service reviews the atom and decides whether it wants to pay for the transaction fee.
8. If the service chooses to pay the cost, it signs the atom and broadcasts it.

## Conclusion

As explained, fee delegation is a good strategy for improving the user experience when interacting with DLTs. We believe that distributed ledgers shouldn't be difficult to transact with.

Beyond this elemental use case for improving usability for first-time users, delegated fees have more applications, such as:

* A DApp requires users to utilize only their native token \(e.g., for paying fees, for governance, voting, etc.\) so users aren't tied to the underlying chain, making migration easier.
* Promoting particular behaviors, like encouraging the users to add rather than remove liquidity on a Decentralized Exchange \(DEX\).
* Increased user privacy since accounts don’t need to be funded for transactions.

