# File Notarization

### Overview

Thanks to the immutability storage and times-stamping capabilities of public DLT, notarization is one of the most commonly referenced use-cases outside of transactional services. This section will describe how to create a basic notarization flow using the Radix ledger:

1. Alice uploads file 
2. File gets notarized 
3. Alice sends file to Bob 
4. Bob uploads file 
5. Bob verifies that Alice was the one who notarized the file

### How it works 

_Note: In this example, we are using a method that is not advisable for very large scale deployments as it also reduces the available_ [_key-space_](https://en.wikipedia.org/wiki/Key_space_%28cryptography%29) _and creates higher probabilities of key clash. This is not a problem for the use case directly, but creates a wider issue for the network if used for billions of files._

_This is not a concern for the Radix main net. There are many ways to do public file notarisation, and as the Radix feature set expands this method will be retired._

Here we take advantage of the fact that a file can be [hashed](https://www.radixdlt.com/post/primer-on-hashes-and-hash-functions) into a [seed](https://en.wikipedia.org/wiki/Random_seed), this seed can then be used to generate a [private/public key-pair](https://www.radixdlt.com/post/primer-on-public-key-cryptography) that maps to an address on the Radix ledger. This simple function means that the same file will always generate the same key pair; allowing anyone with the file to also find the public address.

All transactions in Radix are timestamped, both by [multiple logical clocks](https://docs.radixdlt.com/alpha/learn/platform/tempo#creating-relative-order-of-events-on-the-radix-ledger) and with a [wall-clock timestamp](https://papers.radixdlt.com/tempo/latest/#temporal-proof-provisioning). On the Alpha Net this wall clock time is not consensus validated but it will be on the Beta Network, meaning that timestamps can be trusted to almost the same level as transactions consensus. 

Data transactions therefore both allow the [storage of information on the ledger](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#storing-an-application-payload), which can be easily [read by any client that requests it](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#reading-atoms-from-a-public-address), but also timestamps those transactions in a human readable way.

The following outline uses the publicly available Radix tools, with an imagined simple web application built over the top:

#### Setup: 

Alice has a file that she wants to notarize. She opens a web app with an upload field and uploads the file into it.

#### Step 1:

The web app [hashes](https://www.radixdlt.com/post/primer-on-hashes-and-hash-functions) the file and uses that hash as the deterministic [seed](https://en.wikipedia.org/wiki/Random_seed) for the file’s [Account](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/quick-start#account)/[Identity](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/quick-start#identity) on the ****Radix ledger. The web app then [queries that account address](https://docs.radixdlt.com/alpha/developer/javascript-client-library-guide/code-examples#reading-atoms-from-a-public-address) on the ledger to see whether there is a ‘Claimed’ message going on that account.

#### Step 2: 

If no ‘Claimed’ message is found, the system send its own 'Claimed' message to the Account. 

The 'Claimed' message can be anything Alice wishes it to be; and can either be encrypted using the Account public key, or be left as an unencrypted message. If encrypted using the Account public key, it can be decrypted later by anyone who also has the original file, but not by anyone who does not.

The 'Claimed' message may also include Alice's public key so that she can prove that she was the person who claimed the account at a later date.

#### Step 3: 

Alice shares the file \(plus, optionally, her public key and signature\) with Bob. He performs Steps 0 and 1 as well, but the web app will now return Alice's 'Claimed' message, plus the timestamp of the claiming transaction. If Bob also supplied Alice's public key and signature, it would be simple to verify that Alice was definitely the claimer.

![](https://docs.google.com/a/radixdlt.com/drawings/d/sxvUF-wWqDE_u-KZRgmmk0A/image?w=481&h=335&rev=203&ac=1&parent=1TYT1jod-vt6y-6pW71m5937awQgUaCl73V_9XawF_c4)

This example can be extended by Bob also sending a 'Claimed' message to the document Account, with his public key included. Now both Bob and Alice can both be shown to have copies of the original documents, and on what time/date those copies were notarized.

This example can be further extended by Bob and Alice sending signed messages, from their own Accounts, using their Radix Identities, and encrypted using the document account public key. Now they can also make a simple legal contractual agreement, linked to the notarized document.

### Conclusion 

While being one of the most basic use cases on Radix, only using seeded accounts and messages, it clearly shows how much easier it is to create such dApps on Radix simply using client logic and APIs instead of complex and expensive smart contracts.

In future iterations, we will see how the usage of account ownership, multisignature accounts and other tools can be used to improve the constraints allowing for features such as ownership transfers and its acceptance by third parties.

