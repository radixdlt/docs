# Kotlin Client Library

## Introduction

This library provides access to a [Radix](https://www.radixdlt.com/) Distributed Ledger from Kotlin/Java projects while maximizing compatibility with all versions of Android.

Compatibility with lower versions of Android is achieved by avoiding the use of any Java 8 APIs, e.g. Stream, Optional, Function, etc. and using Kotlin built-in alternatives.

### Code style

The project uses [ktlint](https://github.com/shyiko/ktlint) via [Gradle](https://gradle.org/) dependency.  
To check code style - `gradle ktlint` \(it's also bound to `gradle check`\).

### Features

* Connection to the Alphanet test network
* Fee-less transactions for Testnets
* Identity Creation
* Native token transfers
* Immutable data storage
* Instant Messaging and TEST token wallet DApp implementation
* RXJava 2 based
* Utilizes JSON-RPC over Websockets

## Gradle <a id="gradle"></a>

Include the following Gradle dependency:

```kotlin
repositories {
    maven { url 'https://jitpack.io' }
}​
```

```kotlin
dependencies {
    implementation 'com.radixdlt:radixdlt-kotlin:0.11.3'
}
```

## Identities <a id="identities"></a>

An Identity is the user's credentials \(or more technically the manager of the public/private key pair\) into the ledger, allowing a user to own and send tokens as well as decrypt data.

To create/load an identity from a file:

```kotlin
val identity: RadixIdentity = RadixIdentities.loadOrCreateEncryptedFile("filename.key", "password")
```

This will create a new file which stores the public/private key and encrypted with the given password.

## Universes <a id="universes"></a>

A Universe is an instance of a Radix Distributed Ledger which is defined by a genesis atom and a dynamic set of unpermissioned nodes forming a network.

To bootstrap to the Alphanet test network:

```kotlin
RadixUniverse.bootstrap(Bootstrap.ALPHANET)
```

{% hint style="info" %}
**Note:** No network connections will be made yet until it is required.
{% endhint %}

## Radix DApp API <a id="radix-dapp-api"></a>

The Radix Application API is a client-side API exposing high-level abstractions to make DApp creation easier.

To initialize the API:

```kotlin
RadixUniverse.bootstrap(Bootstrap.ALPHANET) // This must be called before RadixApplicationAPI.create()
val api: RadixApplicationAPI = RadixApplicationAPI.create(identity)
```

### Addresses <a id="addresses"></a>

An address is a reference to an account and allows a user to receive tokens and/or data from other users.

You can get your own address by:

```kotlin
val myAddress: RadixAddress = api.myAddress
```

Or from a base58 string:

```kotlin
val anotherAddress: RadixAddress = RadixAddress.fromString("JHB89drvftPj6zVCNjnaijURk8D8AMFw4mVja19aoBGmRXWchnJ")
```

### Storing and Retrieving Data <a id="storing-and-retrieving-data"></a>

Immutable data can be stored on the ledger. The data can be encrypted so that only selected identities can read the data.

To store the encrypted string `Hello` which only the user can read:

```kotlin
val myPublicKey: ECPublicKey = api.myPublicKeyval
data: Data = Data.DataBuilder().bytes("Hello".toByteArray()).addReader(myPublicKey).build()
result: Result = api.storeData(data, <address>)
```

To store unencrypted data:

```kotlin
val data: Data = Data.DataBuilder().bytes("Hello World".toByteArray()).unencrypted().build()
val result: Result = api.storeData(data, <address>)
```

The returned `Result` object exposes RXJava interfaces from which you can get notified of the status of the storage action:

```kotlin
result.toCompletable().subscribe(<on-success>, <on-error>)
```

To then read \(and decrypt if necessary\) all the readable data at an address:

```kotlin
val readable: Observable<UnencryptedData> = api.getReadableData(<address>)
readable.subscribe { data ->  ...  }
```

{% hint style="info" %}
**Note:** data which is not decryptable by the user's key is simply ignored
{% endhint %}

### Sending and Retrieving Tokens <a id="sending-and-retrieving-tokens"></a>

To send an amount of TEST \(the testnet native token\) from my account to another address:

```kotlin
val result: Result = api.sendTokens(<to-address>, Amount.of(10, Asset.TEST))
```

To retrieve all of the token transfers which have occurred in my account:

```kotlin
val transfers: Observable<TokenTransfer> = api.getMyTokenTransfers(Asset.TEST)
transfers.subscribe { tx -> ... }
```

To get a stream of the balance of TEST tokens in my account:

```kotlin
val balance: Observable<Amount> = api.getMyBalance(Asset.TEST)
balance.subscribe { bal -> ... }
```

## Join the Radix Community

* ​[Telegram](https://t.me/radix_dlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [hello@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.

