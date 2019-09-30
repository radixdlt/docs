# Radix Java card

## Introduction

This document presents a basic implementation of a Java Card applet that is compatible with Radix’s Atom stack and its cryptographic policies. The Radix Card demo applet supports the generation and injection of keys derived from the Secp256k1 curve, signing, verification, hashing of messages, and basic PIN functionality.

Please note that this applet is for demonstration purposes only and is not purposed for commercial deployments nor designed to meet the security requirements of EMV or other standards.

{% hint style="success" %}
**Tip:** if you want to familiarize yourself with the Java smart card technology, visit the [Card platform documentation](https://docs.oracle.com/javacard/).
{% endhint %}

## Java card overview

The Java Card technology combines a subset of the Java programming language with a runtime environment optimized for smart cards and related, small-memory embedded devices. The goal is to bring many of the benefits of Java software programming to the resource-constrained world of smart cards.

The smart card architecture consists of a communication interface, memory, and a CPU for performing calculations and processing information. The card reader, or Card Acceptance Device \(CAD\), serves as a conduit for information into and out of the card.

A Java technology smart card is a smart card that can execute Java Card applets. Keep in mind that these applets run in the Java Card environment, which may be as small as:

* 24K of ROM
* 16K of EEPROM
* 512 bytes of RAM

### APDUs

Smart cards communicate using a packet mechanism called Application Protocol Data Units \(APDUs\). Smart cards are reactive communicators: they never initiate communications, and they only respond to APDUs from the CAD. The communication model is command-response based—that is, the card receives a command APDU, performs the processing requested by the command, and returns a response APDU.

A general APDU communication sequence for smart cards is as follows:

1. Receive an APDU object as a parameter to the process\(\) method.
2. Parse the APDU command header.
3. Read any APDU data.
4. Process the APDU command and generate any response data.
5. Send any response data from the applet.
6. Perform any additional processing, making sure to not change the response APDU buffer.
7. Return from the process\(\) method normally, or throw an ISOException object \(the JCRE appends the appropriate status bytes to the response APDU\).

{% hint style="info" %}
**Note:** The APDU buffer is formatted differently for command APDUs and response APDUs.
{% endhint %}

## Radix implementation

In this section, we present a quick overview of the Radix card's applet methods, explaining their intended purpose and the required parameters.

Please note that the current standard Java Card API doesn't support the signing of a pre-computed message digest, nor perform an internal hash of the message payload to be signed before signing. Therefore we have to transfer the message payload to the card.

A further consideration is that we would like to have our message data "double hashed" to guard against pre-image attacks and other security concerns. The message's data undergo the first round of hashing before signing, and the second round is performed by the signing functions taking advantage of the limitations of the standard API.

{% hint style="success" %}
**Tip:** get the Radix Java card sorce code from our [GitHub repository](https://github.com/radixdlt/radixdlt-card-applet).
{% endhint %}

### Install <a id="docs-internal-guid-2532be69-7fff-4b47-58ad-5e70decc432e"></a>

This method installs the applet into the card. The first thing install\(\) should do is construct the applet by calling its constructor. This has the effect of allocating and initializing any non-static class instance variables that the applet has declared, and instantiating its non-static methods. Static class variables are allocated and initialized before the constructor is called when the applet is loaded onto the card.

{% hint style="info" %}
**Note:** only the class's install method should create the applet object.
{% endhint %}

```java
public static void install(byte[] bArray, short bOffset, byte bLength)
```

Install parameter format is as follows:

* 1 byte declaration of key provisioning type
  * 0 - declares keys are not provided and should be generated
  * 1 - declares keys are provided within install parameters
* 1 byte user PIN length
* n byte user PIN
* 1 byte master PIN length
* n byte master PIN
* 1 byte private key length
* n byte private key
* 1 byte public key length
* n byte public key \(uncompressed\)

### Select

This method is called by the JCRE to indicate that the applet has been selected. It performs necessary initialization which is required to process the subsequent APDU messages.

```java
public boolean select()
```

### Process

This method processes an incoming APDU. After the applet is successfully selected, the JCRE dispatches incoming APDUs to this method. The APDU object is owned and maintained by the JCRE. It encapsulates the details of the underlying transmission protocol by providing a common interface.

```java
public void process(APDU apdu)
```

### Authorise

This method authorises use of the card via either the user or master PIN. In the event that the number of attempts against the user PIN exceeds the retry limit, it can be unblocked via a successful authorisation of the master PIN. Should the number of attempts against the master PIN exceeds the retry limit, the card becomes unusable.

```java
private void authorise(APDU apdu) 
```

* APDU.P1 = 0 - declares a user PIN is provided
* APDU.P1 = 1 - declares a master PIN is provided
* APDU.P2 = UNUSED
* APDU.CDATA = the PIN

### Configure Auth

This method configures the authorisation parameters of the card. Configuring the authorisation parameters of the card requires a successful authorisation of the master PIN. The user or master PIN can then be modified to new values.

```java
private void configureAuth(APDU apdu) 
```

* APDU.P1 = 0 - declares the user PIN is to be modified
* APDU.P1 = 1 - declares the master PIN is to be modified
* APDU.P2 = UNUSED
* APDU.CDATA = the master PIN and new value for the PIN declared in APDU.P1 in the format
  * 1 byte master PIN length
  * n byte master PIN
  * 1 bytes new PIN length
  * n byte new PIN   

### Get Public Key

This method gets the public key from the card.

```java
private void getPublicKey(APDU apdu)
```

*      APDU.P1 = 0 - requests an uncompressed public key
*      APDU.P1 = 1 - requests a compressed public key
*      APDU.P2 = UNUSED
*      APDU.CDATA = UNUSED

### Get Private Key

This method gets the private key from the card. The method requires that a successful authorisation has been performed with the master PIN.

```java
private void getPrivateKey(APDU apdu) 
```

*      APDU.P1 = UNUSED
*      APDU.P2 = UNUSED
*      APDU.CDATA = UNUSED

### Sign Message

This method signs a message payload with the private key on the card. The method requires that a successful authorisation has been performed with either the master or user PIN.

```java
private void signMessage(APDU apdu)
```

*      APDU.P1 = UNUSED
*      APDU.P2 = UNUSED
*      APDU.CDATA = the message payload to sign

{% hint style="info" %}
**Note:** Large message payloads of up to 65KB will utilise the `EXTENDED_APDU` protocol.  If `EXTENDED_APDU` is not supported by the card, an exception will be thrown.
{% endhint %}

### Verify Message

This method verifies that a message payload has been signed with the private key on the card.

```java
private void verifyMessage(APDU apdu)
```

* APDU.P1 = UNUSED
* APDU.P2 = UNUSED
* APDU.CDATA = the signature and message payload to verify in the format
  * 1 byte signature length
  * n byte signature
  * 2 bytes message payload length
  * n byte message payload

{% hint style="info" %}
**Note:** Large message payloads of up to 65KB will utilise the `EXTENDED_APDU` protocol.  If `EXTENDED_APDU` is not supported by the card, an exception will be thrown.
{% endhint %}

### Hash Message

This method hashes a message payload. The message payload is "double hashed" to guard against pre-image attacks and other security concerns.  The message digest produced from the first round is then used as the message payload for the second round.

```java
private void hashMessage(APDU apdu)
```

*      APDU.P1 = UNUSED
*      APDU.P2 = UNUSED
*      APDU.CDATA = the message payload to hash

{% hint style="info" %}
**Note:** Large message payloads of up to 65KB will utilise the `EXTENDED_APDU` protocol.  If `EXTENDED_APDU` is not supported by the card, an exception will be thrown.
{% endhint %}

### Status

This method gets the card version and memory status.

```java
private void status(APDU apdu)
```

*      APDU.P1 = UNUSED
*      APDU.P2 = UNUSED
*      APDU.CDATA = UNUSED

## Source code <a id="docs-internal-guid-a009c672-7fff-52f1-a963-439eb185f28a"></a>

After reading this article, with the provided sample [source code](https://github.com/radixdlt/radixdlt-card-applet) a developer with enough knowledge of the Java Card technology and the Java Card API should be able to develop its own Java smart card capable of interacting with the Radix ledger.

{% hint style="success" %}
**Tip:** get the Radix Java card sorce code from our [GitHub repository](https://github.com/radixdlt/radixdlt-card-applet).
{% endhint %}

{% hint style="info" %}
**Note:** to install the applet onto cards Global Platform Pro can be used using the default AID of - `DEADBEEF7900`
{% endhint %}



