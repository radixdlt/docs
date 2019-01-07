---
description: >-
  The developers guide to building on the Radix Test Network with the Java
  Client Library
---

# Java Client Library

## Features

* Connect to the Alphanet test network
* Fee-less transactions for testnets
* Identity Creation
* Native token transfers
* Immutable data storage
* Instant Messaging and TEST token wallet Dapp implementation
* RXJava 2 based
* Utilizes JSON-RPC over Websockets

## Gradle

Include the following Gradle dependency

```java
repositories {
    maven { url 'https://jitpack.io' }
}
```

```java
dependencies {
    implementation 'com.radixdlt:radixdlt-java:0.11.3'
}
```

## Identities

An Identity is the user's credentials \(or more technically the manager of the public/private key pair\) into the ledger, allowing a user to own tokens and send tokens as well as decrypt data.

To create/load an identity from a file:

```java
RadixIdentity identity = RadixIdentities.loadOrCreateEncryptedFile("filename.key", "password");
```

This will either create or load a file with a public/private key and encrypted with the given password.

## Universes

A Universe is an instance of a Radix Distributed Ledger which is defined by a genesis atom and a dynamic set of unpermissioned nodes forming a network.

To bootstrap to the Alphanet test network:

```java
RadixUniverse.bootstrap(Bootstrap.ALPHANET);
```

NOTE: No network connections will be made yet until it is required.

## Radix Dapp API

The Radix Application API is a client side API exposing high level abstractions to make DAPP creation easier.

To initialize the API:

```java
RadixUniverse.bootstrap(Bootstrap.ALPHANET); // This must be called before RadixApplicationAPI.create()
RadixApplicationAPI api = RadixApplicationAPI.create(identity);
```

### Addresses

An address is a reference to an account and allows a user to receive tokens and/or data from other users.

You can get your own address by:

```java
RadixAddress myAddress = api.getMyAddress();
```

Or from a base58 string:

```java
RadixAddress anotherAddress = RadixAddress.fromString("JHB89drvftPj6zVCNjnaijURk8D8AMFw4mVja19aoBGmRXWchnJ");
```

### Storing and Retrieving Data

Immutable data can be stored on the ledger. The data can be encrypted so that only selected identities can read the data.

To store the encrypted string `Hello` which only the user can read:

```java
ECPublicKey myPublicKey = api.getMyPublicKey();
Data data = new DataBuilder()
    .bytes("Hello".getBytes(StandardCharsets.UTF_8))
    .addReader(myPublicKey)
    .build();
Result result = api.storeData(data, <address>);
```

To store the unencrypted string `Hello`:

```java
Data data = new DataBuilder()
    .bytes("Hello".getBytes(StandardCharsets.UTF_8))
    .unencrypted()
    .build();
Result result = api.storeData(data, <address>);
```

The returned `Result` object exposes RXJava interfaces from which you can get notified of the status of the storage action:

```java
result.toCompletable().subscribe(<on-success>, <on-error>);
```

To then read \(and decrypt if necessary\) all the readable data at an address:

```java
Observable<UnencryptedData> readable = api.getReadableData(<address>);
readable.subscribe(data -> { ... });
```

NOTE: data which is not decryptable by the user's key is simply ignored

### Sending and Retrieving Tokens

To send an amount of TEST \(the testnet native token\) from my account to another address:

```java
Result result = api.sendTokens(<to-address>, Amount.of(10, Asset.TEST));
```

To retrieve all of the token transfers which have occurred in my account:

```java
Observable<TokenTransfer> transfers = api.getMyTokenTransfers(Asset.TEST);
transfers.subscribe(tx -> { ... });
```

To get a stream of the balance of TEST tokens in my account:

```java
Observable<Amount> balance = api.getMyBalance(Asset.TEST);
balance.subscribe(bal -> { ... });
```

## Join the Radix Community

* ​[Telegram](https://t.me/radixdlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [hello@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.



