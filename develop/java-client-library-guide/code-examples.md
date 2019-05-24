# Code examples

In this article, we demonstrate a few implementation examples to execute basic tasks with our Java library.

These code examples are divided into several topics:

* **General**
  * [Initializing a Universe](code-examples.md#initializing-a-universe)
  * [Initializing the DApp API](code-examples.md#initializing-the-dapp-api)
* **Manage atoms**
  * [Signing an atom](code-examples.md#docs-internal-guid-57047dd8-7fff-3eed-f01d-8e2d3fbcf21d)
  * [Submitting an atom to the ledger](code-examples.md#submitting-an-atom-to-the-ledger)
  * [Sending an atom as a message](code-examples.md#sending-an-atom-as-message)
* **Manage identities**
  * [Creating an Identity](code-examples.md#creating-an-identity)
  * [Generating an Address](code-examples.md#generating-an-address)
* **Manage transactions**
  * [Storing data](code-examples.md#storing-data)
  * [Retrieving data](code-examples.md#retrieving-data)
* **Manage tokens**
  * [Creating a token](code-examples.md#docs-internal-guid-3b764101-7fff-8baf-1c30-62c69f61072e)
  * [Minting tokens](code-examples.md#minting-tokens)
  * [Getting a token's resource identifier](code-examples.md#getting-a-tokens-resource-identifier)

{% hint style="success" %}
**Tip:** if you're new to our Java library, we suggest you begin with our [Get Started guide](get-started.md).
{% endhint %}

## General

### Initializing a Universe

To bootstrap to the **ALPHANET** test network:

```java
RadixUniverse.bootstrap(Bootstrap.ALPHANET);
```

To bootstrap to the **BETANET** test network:

```text
RadixUniverse.bootstrap(Bootstrap.BETANET);
```

{% hint style="info" %}
**Note:** No network connections will be made yet until it is required.
{% endhint %}

### Initializing the DApp API

The Radix Application API is a client-side API exposing high-level abstractions to make DApp creation easier. 

To initialize the API:

```java
RadixUniverse.bootstrap(Bootstrap.ALPHANET); // This must be called before RadixApplicationAPI.create()
RadixApplicationAPI api = RadixApplicationAPI.create(identity);
```

## Manage atoms

### Signing an Atom <a id="docs-internal-guid-57047dd8-7fff-3eed-f01d-8e2d3fbcf21d"></a>

To sign an unsigned atom using the current account's signature, use the Identity’s `sign(...)` method:

```java
Atom mySignedAtom = api.getMyIdentity().sign(<my_unsigned_atom>);
```

### Submitting an Atom to the ledger

To submit an atom to the ledger, just use the API’s `submitAtom(...)` method:

```java
api.submitAtom(<my_atom>);
```

### Sending an Atom as message

In the following example, we serialize an atom and send it as a message to a destination address:

```java
String address = “JHB89drvftPj6zVCNjnaijURk8D8AMFw4mVja19aoBGmRXWchnJ”;
byte[] data = atom.toDson();
RadixAddress receiver = RadixAddress.from(address);

api.sendMessage(data, false, receiver);
```

{% hint style="info" %}
**Note:** this method does not submit the atom to the ledger. It only sends it as an arbitrary data byte array. There is no validation performed on the atom.
{% endhint %}

## Manage identities

### Creating an Identity

To create or load an identity from a file:

```java
RadixIdentity identity = RadixIdentities.loadOrCreateEncryptedFile("filename.key", "password");
```

This will either create or load a file with a public/private key and encrypted with the given password.

### Generating an Address

An address is a reference to an account and allows a user to receive tokens and/or data from other users.

You can get your own address by:

```java
RadixAddress myAddress = api.getMyAddress();
```

Or from a base58 string:

```java
RadixAddress anotherAddress = RadixAddress.fromString("JHB89drvftPj6zVCNjnaijURk8D8AMFw4mVja19aoBGmRXWchnJ");
```

## Manage transactions

### Storing data

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

### Retrieving data

To read \(and decrypt if necessary\) all the readable data at an address:

```java
Observable<UnencryptedData> readable = api.getReadableData(<address>);
readable.subscribe(data -> { ... });
```

{% hint style="info" %}
**Note:** data that can't be decrypted by the user's key is simply ignored.
{% endhint %}

## Manage tokens

{% hint style="warning" %}
**NOTE:** these examples requires **BETANET** network access.
{% endhint %}

### Creating a token <a id="docs-internal-guid-3b764101-7fff-8baf-1c30-62c69f61072e"></a>

To create your own token on Radix, you can use the `createToken(...)` method provided by the Java DApp API:

```java
api.createToken(<token_name>, <iso_name>, <description>, 
    BigDecimal.ZERO, TokenUnitConversions.getMinimumGranularity(), 
    TokenSupplyType.MUTABLE);
```

### Minting tokens

When you need to mint new tokens, you can simply call the `mintTokens(...)` method as shown below:

```java
api.mintTokens(<iso_token_name>, <amount_to_mint>);
```

### Getting a token’s Resource Identifier

To get the Radix Resource Identifier \(RRI\) for a token created by a previous call to `createToken(...)`, we have to:

```java
RRI myRRid = RRI.of(api.getMyAddress(), <iso_token_name>);
```

{% hint style="info" %}
**Note:** no validation is made whether the returned RRI actually points to a valid token in the internal address space of the client.
{% endhint %}

