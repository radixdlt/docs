# Alias system \(Live\)

## Introduction

In this article, you’ll learn the basic concepts to build the required methods such as “register an alias” or “resolve an alias” in your own application. You’ll also learn how you can leverage the features of the Radix ledger to implement a simple decentralized Alias system for your app, without having a central server to manage the database index or store the data.**‍**

### **What are aliases in computer systems?**

An alias is an alternate name for someone or something. For instance, Bruce Wayne is known by his pseudonym, Batman. In computer science, an alias is an alternative name for a computer, object, file, device, person, group, or user. Usually, an alias is used to replace long strings with human-readable names. For example, website domain names and usernames are aliases. This allows users to memorize simple URL addresses like `google.com` as opposed to a complex, non-intuitive list of Google servers IP addresses.

### **So why is it important to have an aliasing system on a distributed ledger?**

In distributed ledger technology platforms, each member \(node or wallet\) of the network is identified by its unique public key. It's not easy to remember this long key you are messaging or sending tokens to. This creates friction for the user experience. It is much easier to remember or store human-readable names like usernames and domain names.   
  
Therefore we could say that the primary goal of aliases for distributed ledgers is to resolve human-readable names, like `@name` or `name@namespace`, into machine-readable identifiers, including Radix wallet addresses, nodes, and even content hashes, and/or other identifiers.

## Implementing an Alias system in Radix

As we stated earlier, we want to build a decentralized Alias system for our application, without the need to have a dedicated server to manage the database index. Now let’s see how simple it is to achieve our goal, by interacting with the Radix ledger.

Assume our application wants to provide users with an alias system that supports simple alias \(in the form of `@name`\) and namespace alias \(in the form of `name@namespace`\). The implemented alias logic has to be part of the app client code so all the wallets can interpret it consistently.

First, in our application, we define an [Account](../glossary.md#account) that will be our Namespace index and another Account that will represent our Root index. In the Namespace index, we add a record that points to the Root index so we can handle any simple alias.

![Figure 1: Alias system diagram](https://uploads-ssl.webflow.com/5a5f0fc1f1856e000182ae9e/5bf6a6dd6ffe192f78a81384_Use%20cases_%20aliases%20in%20Radix.jpeg)

The elements of the system shown in the figure:

* **Namespace Index**: a Global index that enables the system to store aliases separately for the root and each namespace.
* **Root Index**: This index is the one that stores the simple aliases.
* **Pointer account**: Each alias on the Root Index links to a single pointer account that links to the users. This will allow users to modify or forward it in the future.
* **Real account**: This is the actual user’s account that is behind the alias.

### **Registering a simple alias**

In our application, when a user wants to register a simple alias, we only have to read from the Namespace index account, verify that there is no namespace already registered with that alias, find the reference to the Root index account, and add a record in the Root for the requested user alias.

That alias record will point to a new Account used as a pointer reference, where we store the Alias owner, their current address, and any other relevant information. Note that the pointer account can be later updated only by the Alias owner, in case he needs to change any information.

As you might wonder, there’s no actual need to check for availability or duplicates, as alias is assigned in a first-come-first-served basis, and even if someone tries to register a name twice, the immutable ledger allows us to know who has the first valid registration. 

### **Registering a namespace alias**

In a similar way, when a user wants to register a namespace alias such as `name@myapp`, we have to read from the Namespace index account, find the reference to the requested namespace index Account, and then add a record in the `myapp` index for the requested user alias.

Depending on your application-specific needs, a namespace alias could be modified by the owner of the namespace \(like email domains\) or only by the alias owner \(like a simple alias\).

### **Resolving an alias**

Let’s review how we can resolve aliases so we can have a complete alias system in our application.

When we want to resolve an alias, we start by reading the Namespace index records, to find the right index for the alias. For a simple alias that would be the Root index, and for a namespace alias would be the defined namespace index. In case it’s a simple alias, we start reading the Root index records, and we take the first match we find, to access the alias Pointer account. 

We continue by reading the first record of the alias pointer to find the alias owner and keep reading any other record in the Pointer account to see if the alias had any additional update by the alias owner. Once we reach the end of the pointer data, we have the latest Account address for the simple alias. Instead, if it’s a namespace alias, next, we start reading the defined namespace index records, and we take the first match we find. 

In this scenario, the process is simplified, and the first match we find in the private namespace index is the Account address for the alias.

## Next steps

Learn the basics of interacting with the Radix ledger, and build a decentralized alias system using your preferred client library from below:

* [JavaScript Client Library](https://docs.radixdlt.com/radixdlt-js/) 
* [Java Claent Library](https://docs.radixdlt.com/radixdlt-java/)

