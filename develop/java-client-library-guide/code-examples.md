# Code examples

In this article we demonstrate a few implementation examples to execute basic tasks with our Java library.

These code examples are divided into several topics:

* **General**
  * [Initializing a Universe](code-examples.md#initializing-a-universe)
* **Manage identities**
  * [Creating an Identity](code-examples.md#creating-an-identity)
* **Manage transactions**
  * [Storing and retrieving data](code-examples.md#storing-and-retrieving-data)

{% hint style="success" %}
**Tip:** if you're new to our Java library, we suggest you to begin with our [Get Started guide](get-started.md).
{% endhint %}

## General

### Initializing a Universe

To bootstrap to the **ALPHANET** test network:

```java
RadixUniverse.bootstrap(Bootstrap.ALPHANET);
```

{% hint style="info" %}
**Note:** No network connections will be made yet until it is required.
{% endhint %}

## Manage identities

### Creating an Identity

To create/load an identity from a file:

```java
RadixIdentity identity = RadixIdentities.loadOrCreateEncryptedFile("filename.key", "password");
```

This will either create or load a file with a public/private key and encrypted with the given password.

## Manage transactions

### Storing and retrieving data

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

{% hint style="info" %}
**Note:** data that can't be decrypted by the user's key is simply ignored.
{% endhint %}

