# Code examples

In this article, we demonstrate a few implementation examples to execute basic tasks with our JavaScript library.

These code examples are divided into several topics:

* **General**
  * [Initializing a universe](code-examples.md#initializing-a-universe)
  * [Setting a log level](code-examples.md#setting-a-log-level)
* **Manage accounts**
  * [Creating a custom account system](code-examples.md#creating-a-custom-account-system)
* **Manage atoms**
  * [Reading atoms from a public address](code-examples.md#reading-atoms-from-a-public-address)
  * [Reading and decrypting atoms from an owned address](code-examples.md#reading-and-decrypting-atoms-from-an-owned-address)
  * [Caching atoms](code-examples.md#caching-atoms)
* **Manage identities**
  * [Creating a remote identity](code-examples.md#creating-a-remote-identity)
  * [Creating a simple identity](code-examples.md#creating-a-simple-identity)
* **Manage transactions**
  * [Sending a transaction](code-examples.md#sending-a-transaction)
  * [Sending a message](code-examples.md#sending-a-message)
  * [Storing an application payload](code-examples.md#storing-an-application-payload)
* **Manage private keys**
  * [Storing private keys](code-examples.md#storing-private-keys)
  * [Loading private keys](code-examples.md#loading-private-keys)

{% hint style="success" %}
**Tip:** if you're new to our JS library, we suggest you begin with our [Get Started guide](quick-start.md).
{% endhint %}

## General

* [Initializing a universe](code-examples.md#initializing-a-universe)
* [Setting a log level](code-examples.md#setting-a-log-level)

### Initializing a universe

There are different Universes available, such as `ALPHANET`, `HIGHGARDEN`_,_ and `SUNSTONE`. Typically, for development purposes we use **ALPHANET**.

To bootstrap to a network we call:

```javascript
radixUniverse.bootstrap(RadixUniverse.ALPHANET)
```

### Setting a log level

In the following code snippet we set the log level to display errors only:

```javascript
import { RadixLogger } from 'radixdlt'

RadixLogger.setLevel('error')
```

{% hint style="info" %}
Possible log level values are: `trace`, `debug`, `info`, `warn`, `error`.
{% endhint %}

## Manage atoms

Let's review some code examples on how to manage **Atoms**:

* [Reading atoms from a public address](code-examples.md#reading-atoms-from-a-public-address)
* [Reading and decrypting atoms from an owned address](code-examples.md#reading-and-decrypting-atoms-from-an-owned-address)
* [Caching atoms](code-examples.md#caching-atoms)

### Reading atoms from a public address

In the following code snippet, we read **Atoms** from the public address `9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM`, by opening a **Node** connection and subscribing to the transaction updates.

```javascript
const account = RadixAccount.fromAddress('9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM')
account.openNodeConnection()
​
account.transferSystem.balance // This is the account balance
account.transferSystem.transactions // This is a list of transactions 
​
// Subscribe for any new incoming transactions
account.transferSystem.transactionSubject.subscribe(transactionUpdate => {
  console.log(transactionUpdate)
})
​
// Subscribe for all previous transactions as well as new ones
account.transferSystem.getAllTransactions().subscribe(transactionUpdate => {
  console.log(transactionUpdate)
})
```

### Reading and decrypting atoms from an owned address

In the following code snippet, we read and decrypt **Atoms** from an owned address, by opening a **Node** connection and getting the application data from `my-test-application`.

```javascript
const identityManager = new RadixIdentityManager()
const identity = identityManager.generateSimpleIdentity()
​
// Each identity comes with an account, which works the same as any account, but can also decrypt encrypted messages
const account = identity.account
​
account.openNodeConnection() 
​
// A list of Radix chat messages in the order they're received
account.messagingSystem.messages 

// Radix chat messages grouped by the other address
account.messagingSystem.chats 

// Subscribe for incoming messages
account.messagingSystem.messageSubject.subscribe(...)

// Subscribe for all previous messages as well as new ones
account.messagingSystem.getAllMessages().subscribe(...)
​
// Custom application data 
account.dataSystem.applicationData.get('my-test-application')

// Subscribe for all incoming application data
account.dataSystem.applicationDataSubject.subscribe(...)

// Subscribe for all previous messages as well as new ones
account.dataSystem.getApplicationData('my-test-application').subscribe(...)

// Subscribe for all previous messages as well as new ones signed by a specific address or more
account.dataSystem.getApplicationData('my-test-application', ['9iKq87ZvC1pdYC26qem4WQTtofNRje9c133vAvBgQnRdpAsHExR']).subscribe(...)
```

### Caching atoms

In the following code snippet, we cache **Atoms** from the public address `9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM`, by defining a `'path/to/file'` and enabling the account's cache.

```javascript
import {RadixNEDBAtomCache} from 'radixdlt'
​
const account = RadixAccount.fromAddress('9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM')
const atomCache = new RadixNEDBAtomCache('path/to/file')
account.enableCache(atomCache) // This will read all atoms in cache, as well as store new ones in the future
​
account.openNodeConnection()
```

## Manage accounts

Let's review some code examples on how to manage **Accounts**:

* [Creating a custom account system](code-examples.md#creating-a-custom-account-system)

### Creating a custom account system

In the following code snippet, we create a custom **Account system**, that enables us to process **Atoms** in any way we want.

```javascript
import { RadixAccountSystem, RadixAtomUpdate, RadixAtom } from 'radixdlt'
​
export default class CustomAccountSystem implements RadixAccountSystem {
  public name: 'CUSTOM'
  
  public async processAtomUpdate(atomUpdate: RadixAtomUpdate) {
    const atom: RadixAtom = atomUpdate.atom
    
    // Do whatever you want with your atom here
    console.log(atom.getHID().toString())
  }
}
```

{% hint style="info" %}
**Note:** the `atomUpdate` has an action field, which can be **STORE** or **DELETE**.

A **DELETE** can occur when an **Atom** fails to validate or is rejected by the consensus. It's crucial to handle it correctly in the particular context of the application.
{% endhint %}

Once you have your custom account system, you can simply add it to the account using the _addAccountSystem\(\)_ method:

```javascript
const account = RadixAccount.fromAddress('9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM')
const myAccountSystem = new CustomAccountSystem()
​
account.addAccountSystem(myAccountSystem)
account.openNodeConnection()
```

## Manage identities

Let's review some code examples on how to manage **Identities**:

* [Creating a remote identity](code-examples.md#creating-a-remote-identity)
* [Creating a simple identity](code-examples.md#creating-a-simple-identity)

{% hint style="success" %}
**Tip:** using a _RemoteIdentity_ allows the JavaScript application to access an existing account that the user already has on a Wallet while keeping the private keys secure.
{% endhint %}

### Creating a simple identity

{% hint style="info" %}
**Note:** a _SimpleIdentity_ keeps both public and private keys in memory.
{% endhint %}

In the following code example, we create a new simple identity using the `RadixIdentityManager`'s generateSimpleIdentity\(\) method: 

```javascript
const identityManager = new RadixIdentityManager()
const myIdentity = identityManager.generateSimpleIdentity()  
```

With it, we can easily access our own Account using the `account` reference: 

```javascript
const myAccount = myIdentity.account​ ​ 
console.log('My account address: ', myAccount.getAddress())
```

### Creating a remote identity

{% hint style="info" %}
**Note:** a _RemoteIdentity_ only holds the public key in memory, while a Wallet keeps the private key. The user must accept a request from the JavaScript application to allow the Wallet to sign atoms on its behalf.
{% endhint %}

In the following code snippet, we try to create a new remote **Identity** and catch any errors in the console log:

```javascript
RadixRemoteIdentity.createNew('my dApp', 'my dApp description').then(identity => {
    //Do stuff with identity here
}).catch(error => {
    console.log(error)
})
```

We can also specify a remote host and port for the remote identity:

```javascript
RadixRemoteIdentity.createNew('my dApp', 'my dApp description', '192.168.0.123', '53433').then(identity => {
    //Do stuff with identity here
}).catch(error => {
    console.log(error)
})
```

In case we want to validate if the remote wallet is up and running:

```javascript
RadixRemoteIdentity.isServerUp().then(isServerUp => {
    if (isServerUp) {
         // Do something
    }
})
```

As an alternative, we can also generate a _RemoteIdentity_ using `RadixIdentityManager`:

```javascript
const identityManager = new RadixIdentityManager()

identityManager.generateRemoteIdentity('my dApp', 'my dApp description').then(identity => {
    //Do stuff with identity here
}).catch(error => {
    console.log(error)
})
```

## Manage transactions

Let's review a few code examples related to _RadixTransactionBuilder_, and see how we can handle transactions, messages, and payloads:

* [Sending a transaction](code-examples.md#sending-a-transaction)
* [Sending a message](code-examples.md#sending-a-message)
* [Storing an application payload](code-examples.md#storing-an-application-payload)

### Sending a transaction

In the following code snippet, we send a transaction from an owned address to the public address `9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM`, by creating a transfer **Atom** and signing it with our **Identity**. Finally, we get the results by subscribing to the transaction updates.

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
myAccount.openNodeConnection()
​
// Wait for the account to sync data from the ledger
​
// No need to load data from the ledger for the recipient account
const toAccount = RadixAccount.fromAddress('9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM', true)
const token = 'TEST' // The Radix TEST token
const amount = 123.12
​
const transactionStatus = RadixTransactionBuilder
  .createTransferAtom(myAccount, toAccount, token, amount)
  .signAndSubmit(myIdentity)
                    
transactionStatus.subscribe({
  next: status => {
    console.log(status) 
    // For a valid transaction, this will print, 'FINDING_NODE', 'GENERATING_POW', 'SIGNING', 'STORE', 'STORED'
  },
  complete: () => { console.log('Transaction complete') },
  error: error => { console.error('Error submitting transaction', error) }
})
```

### Sending a message

In the following code snippet, we send a **Message** from an owned address to the public address `9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM`, by creating a message **Atom** and signing it with our **Identity**. Finally, we get the results by subscribing to the transaction updates.

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
myAccount.openNodeConnection()
​
// Wait for the account to sync data from the ledger
​
// No need to load data from the ledger for the recipient account
const toAccount = RadixAccount.fromAddress('9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM', true)
​
const message = 'Hello World!'
​
const transactionStatus = RadixTransactionBuilder
  .createRadixMessageAtom(myAccount, toAccount, message)
  .signAndSubmit(myIdentity)
                    
transactionStatus.subscribe({
  next: status => {
    console.log(status) 
    // For a valid transaction, this will print, 'FINDING_NODE', 'GENERATING_POW', 'SIGNING', 'STORE', 'STORED'
  },
  complete: () => { console.log('Transaction complete') },
  error: error => { console.error('Error submitting transaction', error) }
})
```

### Storing an application payload

In the following code snippet, we store a **Payload** to the application _my-test-app_ for an owned address and the public address `9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM`, by creating a payload **Atom** and signing it with our **Identity**. Finally, we get the results by subscribing to the transaction updates.

```javascript
const myIdentity = identityManager.generateSimpleIdentity()
const myAccount = myIdentity.account
myAccount.openNodeConnection()
​
// Wait for the account to sync data from the ledger
​
// No need to load data from the ledger for the recipient account
const toAccount = RadixAccount.fromAddress('9i9hgAyBQuKvkw7Tg5FEbML59gDmtiwbJwAjBgq5mAU4iaA1ykM', true)
​
const applicationId = 'my-test-app'
const payload = JSON.stringify({
  message: 'Hello World!',
  otherData: 123
})
​
const transactionStatus = RadixTransactionBuilder
  .createPayloadAtom([myAccount, toAccount], applicationId, payload)
  .signAndSubmit(myIdentity)
                    
transactionStatus.subscribe({
  next: status => {
    console.log(status) 
    // For a valid transaction, this will print, 'FINDING_NODE', 'GENERATING_POW', 'SIGNING', 'STORE', 'STORED'
  },
  complete: () => { console.log('Transaction complete') },
  error: error => { console.error('Error submitting transaction', error) }
})
```

## Manage private keys

Finally, let's review two code examples on how to store and load private keys:

* [Storing private keys](code-examples.md#storing-private-keys)
* [Loading private keys](code-examples.md#loading-private-keys)

### Storing private keys

In the following code snippet, we encrypt the private key of an identity using the password `SuperDuperSecretPassword`. The resulting JSON object can be saved to a file, or in a browser's local storage.

```javascript
const identity = identityManager.generateSimpleIdentity()
​
const password = 'SuperDuperSecretPassword'
RadixKeyStore.encryptKey(identity.keyPair, password).then((encryptedKey) => {
  console.log('Private key encrypted')
}).catch((error) => {
  console.error('Error encrypting private key', error)
})
```

{% hint style="info" %}
The key storage format is compatible with the Radix Desktop and Android wallet applications.
{% endhint %}

### Loading private keys

In the following code snippet, we decrypt a private key using `SuperDuperSecretPassword` as the decryption password.

```javascript
const encryptedKey = loadKeyFromStorage() // This object is what you get from RadixKeyStore.encryptKey(...)
const password = 'SuperDuperSecretPassword'
RadixKeyStore.decryptKey(encryptedKey, password).then((keyPair) => {
  console.log('Private key successfuly decrypted')
​
  const identity = new RadixSimpleIdentity(keyPair)
}).catch((error) => {
  console.error('Error decrypting private key', error)
})
```

