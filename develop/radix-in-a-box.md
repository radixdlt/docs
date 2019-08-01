# Radix-In-A-Box

## Introduction  <a id="introduction"></a>

{% hint style="danger" %}
Please note that Radix-In-A-Box has not yet been released.
{% endhint %}

This is a quick start guide aimed for technical users, to run the Radix-In-A-Box system on your computer.

{% hint style="info" %}
### Radix-In-A-Box limitations

* Radix-In-A-Box in a stand-alone application.
* There is no network or connection between nodes.
* Throughput is limited, as all workload is concentrated on a single node.
* Network API endpoints always return an empty set.
{% endhint %}

## Pre-requisites  <a id="pre-requisites"></a>

### Hardware  <a id="hardware"></a>

Your host computer should have **at least**:

* 2 CPU cores
* 8 GB memory
* 20 GB available disk

{% hint style="info" %}
**Note:** the actual disk size requirement will grow over time as the ledger grows.
{% endhint %}

### Software  <a id="software"></a>

You can run Radix-In-A-Box on any operating system that supports Java 1.8, including:

* Linux
* MacOS X
* Windows 10

The following software is required to run Radix-In-A-Box:

* Git (2.20 is known to work, but any recent version should be fine)
* Java JDK 1.8 (either OpenJDK or Oracle JDK will work)

## Cloning the Repository  <a id="cloning-repository"></a>

Open up a Terminal \(Mac/Linux\) or a Command Prompt \(on Windows\). Navigate to a suitable directory and clone the required repositories:

```bash
$ git clone https://github.com/radixdlt/radix-in-a-box.git
$ git clone https://github.com/radixdlt/radixdlt-engine.git
```

## Running Radix-In-A-Box <a id="running-radixinabox"></a>

From the top-level of the cloned repositories, you can launch the application by navigating into the `radix-in-a-box` directory:

```bash
$ cd radix-in-a-box
$ ./gradlew run
```

Once the application has launched, you should see output similar to the following:

```text
Insert representative output here when available.
```

### Stopping Radix-In-A-Box  <a id="stopping-radixinabox"></a>

In the Terminal or Command Prompt where Radix-In-A-Box is running, type <kbd>Control-C</kbd>.  This will terminate the application.

## Development libraries

Last, but not least, you can start your own DApp development using our [Java](https://docs.radixdlt.com/radixdlt-java/) and [JavaScript](https://docs.radixdlt.com/radixdlt-js/) client libraries:

## Join the Radix Community

* ​[Telegram](https://t.me/radix_dlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [info@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.
