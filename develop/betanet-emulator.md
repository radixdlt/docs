# Betanet Emulator

## Introduction  <a id="introduction"></a>

This is a quick start guide aimed for non-technical users, to run a Radix DLT Betanet Emulator on your computer. The following steps are covered:

1. [Installing Docker](../#installing-docker)
2. [Creating a configuration file](../#creating-a-docker-composeyml-configuration-file)
3. [Launching the Radix DLT Betanet Emulator](../#launching-emulator)

{% hint style="info" %}
### Emulator limitations

* The betanet emulator works in a stand-alone mode.
* There is no network or connection between nodes.
* Throughput is limited, as all workload is concentrated on a single node.
* Network API endpoints always return an empty set.
{% endhint %}

## Pre-requisites  <a id="pre-requisites"></a>

### Hardware  <a id="hardware"></a>

Your host computer should have **at least**:

* 2 CPU cores
* 8 GB memory
* 20 GB disk

{% hint style="info" %}
**Note:** the actual disk size requirement will grow over time as the ledger grows.
{% endhint %}

### Software  <a id="software"></a>

You can run the Radix DLT Betanet Emulator on any operating system that supports Docker and Docker Compose, including:

* Linux
* MacOS X
* Windows 10

## Installing Docker  <a id="installing-docker"></a>

You can download the right Docker Engine \(Community Edition\) for your system here: [https://hub.docker.com/search/?type=edition&offering=community](https://hub.docker.com/search/?type=edition&offering=community)

For a detailed Docker setup guide, please refer to the official [installation documentation](https://docs.docker.com/install/) for [Docker CE](https://www.docker.com/products/docker-engine).

### Installing Docker Compose \(Linux only\)

If you are running Linux, after you completed the Docker setup you need to install [Docker Compose](https://docs.docker.com/compose/install/). See the official Docker Composite [installation guide](https://docs.docker.com/compose/install/) for download links and additional information.

{% hint style="success" %}
**Tip:** Docker Compose is bundled with Docker CE for the Mac and Windows versions.
{% endhint %}

## Creating a configuration file  <a id="creating-a-docker-composeyml-configuration-file"></a>

The Radix DLT software stack is composed of a single docker image, `radixdlt/radixdlt-core:emulator-1.0.0-beta`.

Your `betanet-emulator.yml` determines the software components you will run. In particular, the following settings are specified:

* The Docker image to download and start
* Persistent data volumes - that survive restarts and upgrades.

Start with this docker compose file:

{% code-tabs %}
{% code-tabs-item title="betanet-emulator.yml" %}
```yaml
version: '2.2'
services:
  core:
    image: radixdlt/radixdlt-core:emulator-1.0.0-beta
    init: true
    ports:
      - "8080:8080"
    environment:
      CORE_NETWORK_SEEDS: core
      CORE_SECURE_RANDOM_SOURCE: /dev/urandom
      CORE_UNIVERSE: v2djcmVhdG9yWCIBA3hanCWf3pmR5E+i+wtWWfKleBrDOQduLb/vcFKOSt9oa2Rlc2NyaXB0aW9ueB5UaGUgUmFkaXggZGV2ZWxvcG1lbnQgVW5pdmVyc2VnZ2VuZXNpc4G/aG1ldGFEYXRhv2l0aW1lc3RhbXBtMTU1MTIyNTYwMDAwMP9ucGFydGljbGVHcm91cHODv2lwYXJ0aWNsZXOBv2hwYXJ0aWNsZb9lYnl0ZXNXAVJhZGl4Li4uIGp1c3QgaW1hZ2luZSFsZGVzdGluYXRpb25zgVECVqurOHBYXwTQFdVa32ALx2Rmcm9tWCcEAgN4Wpwln96ZkeRPovsLVlnypXgawzkHbi2/73BSjkrfaIh5wbllbm9uY2UbAAQdARdF7ABqc2VyaWFsaXplcndyYWRpeC5wYXJ0aWNsZXMubWVzc2FnZWJ0b1gnBAIDeFqcJZ/emZHkT6L7C1ZZ8qV4GsM5B24tv+9wUo5K32iIecG5Z3ZlcnNpb24YZP9qc2VyaWFsaXplcnNyYWRpeC5zcHVuX3BhcnRpY2xlZHNwaW4BZ3ZlcnNpb24YZP9qc2VyaWFsaXplcnRyYWRpeC5wYXJ0aWNsZV9ncm91cGd2ZXJzaW9uGGT/v2lwYXJ0aWNsZXODv2hwYXJ0aWNsZb9sZGVzdGluYXRpb25zgVECVqurOHBYXwTQFdVa32ALx2Vub25jZQBjcnJpWDkGL0pIMVA4ZjN6bmJ5ckRqOEY0UldwaXg3aFJrZ3hxSGpkVzJmTm5LcFIzdjZ1Zlhua25vci9YUkRqc2VyaWFsaXplcnNyYWRpeC5wYXJ0aWNsZXMucnJpZ3ZlcnNpb24YZP9qc2VyaWFsaXplcnNyYWRpeC5zcHVuX3BhcnRpY2xlZHNwaW4gZ3ZlcnNpb24YZP+/aHBhcnRpY2xlv2dhZGRyZXNzWCcEAgN4Wpwln96ZkeRPovsLVlnypXgawzkHbi2/73BSjkrfaIh5wblrZGVzY3JpcHRpb25zUmFkaXggTmF0aXZlIFRva2Vuc2xkZXN0aW5hdGlvbnOBUQJWq6s4cFhfBNAV1VrfYAvHa2dyYW51bGFyaXR5WCEFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFnaWNvblVybHg0aHR0cHM6Ly9hc3NldHMucmFkaXhkbHQuY29tL2ljb25zL2ljb24teHJkLTMyeDMyLnBuZ2RuYW1lZFJhZHNrcGVybWlzc2lvbnO/ZGJ1cm5kbm9uZWRtaW50c3Rva2VuX2NyZWF0aW9uX29ubHn/anNlcmlhbGl6ZXJ4IHJhZGl4LnBhcnRpY2xlcy50b2tlbl9kZWZpbml0aW9uZnN5bWJvbGNYUkRndmVyc2lvbhhk/2pzZXJpYWxpemVyc3JhZGl4LnNwdW5fcGFydGljbGVkc3BpbgFndmVyc2lvbhhk/79ocGFydGljbGW/ZmFtb3VudFghBf//////////////////////////////////////////bGRlc3RpbmF0aW9uc4FRAlarqzhwWF8E0BXVWt9gC8drZ3JhbnVsYXJpdHlYIQUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAWVub25jZRsABB0BF0ZbTmtwZXJtaXNzaW9uc79kYnVybmRub25lZG1pbnRzdG9rZW5fY3JlYXRpb25fb25sef9qc2VyaWFsaXplcngicmFkaXgucGFydGljbGVzLnVuYWxsb2NhdGVkX3Rva2Vuc3gYdG9rZW5EZWZpbml0aW9uUmVmZXJlbmNlWDkGL0pIMVA4ZjN6bmJ5ckRqOEY0UldwaXg3aFJrZ3hxSGpkVzJmTm5LcFIzdjZ1Zlhua25vci9YUkRndmVyc2lvbhhk/2pzZXJpYWxpemVyc3JhZGl4LnNwdW5fcGFydGljbGVkc3BpbgFndmVyc2lvbhhk/2pzZXJpYWxpemVydHJhZGl4LnBhcnRpY2xlX2dyb3VwZ3ZlcnNpb24YZP+/aXBhcnRpY2xlc4O/aHBhcnRpY2xlv2ZhbW91bnRYIQX//////////////////////////////////////////2xkZXN0aW5hdGlvbnOBUQJWq6s4cFhfBNAV1VrfYAvHa2dyYW51bGFyaXR5WCEFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFlbm9uY2UbAAQdARdGW05rcGVybWlzc2lvbnO/ZGJ1cm5kbm9uZWRtaW50c3Rva2VuX2NyZWF0aW9uX29ubHn/anNlcmlhbGl6ZXJ4InJhZGl4LnBhcnRpY2xlcy51bmFsbG9jYXRlZF90b2tlbnN4GHRva2VuRGVmaW5pdGlvblJlZmVyZW5jZVg5Bi9KSDFQOGYzem5ieXJEajhGNFJXcGl4N2hSa2d4cUhqZFcyZk5uS3BSM3Y2dWZYbmtub3IvWFJEZ3ZlcnNpb24YZP9qc2VyaWFsaXplcnNyYWRpeC5zcHVuX3BhcnRpY2xlZHNwaW4gZ3ZlcnNpb24YZP+/aHBhcnRpY2xlv2ZhbW91bnRYIQX///////////////////////////zE0cNgL3/DF////2xkZXN0aW5hdGlvbnOBUQJWq6s4cFhfBNAV1VrfYAvHa2dyYW51bGFyaXR5WCEFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFlbm9uY2UbAAQdARdG1htrcGVybWlzc2lvbnO/ZGJ1cm5kbm9uZWRtaW50c3Rva2VuX2NyZWF0aW9uX29ubHn/anNlcmlhbGl6ZXJ4InJhZGl4LnBhcnRpY2xlcy51bmFsbG9jYXRlZF90b2tlbnN4GHRva2VuRGVmaW5pdGlvblJlZmVyZW5jZVg5Bi9KSDFQOGYzem5ieXJEajhGNFJXcGl4N2hSa2d4cUhqZFcyZk5uS3BSM3Y2dWZYbmtub3IvWFJEZ3ZlcnNpb24YZP9qc2VyaWFsaXplcnNyYWRpeC5zcHVuX3BhcnRpY2xlZHNwaW4BZ3ZlcnNpb24YZP+/aHBhcnRpY2xlv2dhZGRyZXNzWCcEAgN4Wpwln96ZkeRPovsLVlnypXgawzkHbi2/73BSjkrfaIh5wblmYW1vdW50WCEFAAAAAAAAAAAAAAAAAAAAAAAAAAADOy48n9CAPOgAAABsZGVzdGluYXRpb25zgVECVqurOHBYXwTQFdVa32ALx2tncmFudWxhcml0eVghBQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABZW5vbmNlGwAEHQEXRsX0a3Blcm1pc3Npb25zv2RidXJuZG5vbmVkbWludHN0b2tlbl9jcmVhdGlvbl9vbmx5/2ZwbGFuY2saAYp/QGpzZXJpYWxpemVyeCRyYWRpeC5wYXJ0aWNsZXMudHJhbnNmZXJyYWJsZV90b2tlbnN4GHRva2VuRGVmaW5pdGlvblJlZmVyZW5jZVg5Bi9KSDFQOGYzem5ieXJEajhGNFJXcGl4N2hSa2d4cUhqZFcyZk5uS3BSM3Y2dWZYbmtub3IvWFJEZ3ZlcnNpb24YZP9qc2VyaWFsaXplcnNyYWRpeC5zcHVuX3BhcnRpY2xlZHNwaW4BZ3ZlcnNpb24YZP9qc2VyaWFsaXplcnRyYWRpeC5wYXJ0aWNsZV9ncm91cGd2ZXJzaW9uGGT/anNlcmlhbGl6ZXJqcmFkaXguYXRvbWpzaWduYXR1cmVzv3ggNTZhYmFiMzg3MDU4NWYwNGQwMTVkNTVhZGY2MDBiYze/YXJYIQGN+yS2lSWiWm+Qp+RE0XKZZlDCmsYEHqVOgwi5sumVH2FzWCEBUZbY2D5H9/zBPnDtU80qy4A1/EAJyrzw/bOCrHlNgoFqc2VyaWFsaXplcnZjcnlwdG8uZWNkc2Ffc2lnbmF0dXJlZ3ZlcnNpb24YZP//bXRlbXBvcmFsUHJvb2a/Y2FpZFghCEJOW9hqiqRdHgAFw1rMG1kA7xFc89evO1arqzhwWF8EanNlcmlhbGl6ZXJ0dGVtcG8udGVtcG9yYWxfcHJvb2ZndmVyc2lvbhhkaHZlcnRpY2Vzgb9lY2xvY2sBamNvbW1pdG1lbnRYIQMAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGVvd25lclgiAQO5kZ9TtqJE8PAqMAkR3IZE413Vm/wmXBPBKjvYm8ddX2hwcmV2aW91c1ECAAAAAAAAAAAAAAAAAAAAAGZyY2xvY2sbAAABaSxAOABqc2VyaWFsaXplcnV0ZW1wby50ZW1wb3JhbF92ZXJ0ZXhpc2lnbmF0dXJlv2FyWCEBPkk2kYaH4ixggyRYaSAYGe80qCxej1B5I+Zi+x/jn4Fhc1ghASXlWsUP3WFDhORmhokZUIVw5sy27TBsZ/Rudr1ueAXkanNlcmlhbGl6ZXJ2Y3J5cHRvLmVjZHNhX3NpZ25hdHVyZWd2ZXJzaW9uGGT/anRpbWVzdGFtcHO/Z2RlZmF1bHQbAAABaSxAOAD/Z3ZlcnNpb24YZP//Z3ZlcnNpb24YZP9lbWFnaWM6T2h//WRuYW1lbFJhZGl4IERldm5ldGZwbGFuY2sZ6mBkcG9ydBl1MGpzZXJpYWxpemVybnJhZGl4LnVuaXZlcnNla3NpZ25hdHVyZS5yWCEBnNHHy2ZNs02RhJ2PaGE8TYjJ6a4JZbs5W5ICDG2FZ4prc2lnbmF0dXJlLnNYIQEzwSczZGOPPINsytlykDyITA51ICNl/GVmTSRwoD4Zeml0aW1lc3RhbXAbAAABaSxAOABkdHlwZQJndmVyc2lvbhhk/w==
      JAVA_OPTS: -server -Xms2g -Xmx2g
    volumes:
      - "core_ledger:/opt/radixdlt/RADIXDB"
      - "core_config:/opt/radixdlt/etc"
    logging:
      options:
        max-size: "10m"
        max-file: "30"
    networks:
      - single_node
  faucet:
    image: radixdlt/faucet:1.0.0-beta
    init: true
    environment:
      FAUCET_DELAY: 0
      FAUCET_TOKEN_RRI: /JH1P8f3znbyrDj8F4RWpix7hRkgxqHjdW2fNnKpR3v6ufXnknor/XRD
      RADIX_IDENTITY_UNENCRYPTED_KEY: JI41zTPZ+DW0JMUBlLciuyrSqT0/Gj9/Oz3+J9MhNL4=
      RADIX_BOOTSTRAP_TRUSTED_NODE: http://core:8080
      JAVA_OPTS: -server
    logging:
      options:
        max-size: "10m"
        max-file: "30"
    networks:
      - single_node
volumes:
  core_ledger:
  core_config:
networks:
  single_node:
```
{% endcode-tabs-item %}
{% endcode-tabs %}

1. Create a directory on your computer for storing docker compose files \(e.g., `radixdlt`\).
2. Use your favorite text editor to create the `betanet-emulator.yml`.
3. Copy and paste the content above.

## Launching the Betanet Emulator  <a id="launching-emulator"></a>

Open up a Terminal \(Mac/Linux\) or a Command Prompt \(on Windows\). Navigate to the directory where you've placed your `betanet-emulator.yml` file. Launch the Docker containers with:

```bash
cd ~/radixdlt
docker-compose -p radixdlt -f betanet-emulator.yml up -d
```

If successful, it should pull down and look something like this when completed:

```text
Creating network "radixdlt_single_node" with the default driver
Creating volume "radixdlt_core_ledger" with default driver
Creating volume "radixdlt_core_config" with default driver
Pulling core (radixdlt/radixdlt-core:emulator-1.0.0-beta)...
emulator-1.0.0-beta: Pulling from radixdlt/radixdlt-core
c87736221ed0: Pull complete
a0d980c21713: Pull complete
7153696eb942: Pull complete
5a8ad49b35c2: Pull complete
6e8ff60c7fe2: Pull complete
e5bd13ba5d60: Pull complete
dc2a7e34f4b6: Pull complete
9520c49c4dca: Pull complete
Creating radixdlt_core_1 ... done
```

Check that you have a `radixdlt_core_1` Docker container with:

```bash
docker ps
```

You can also check if the Betanet Emulator is up and running at [http://localhost:8080/api/system](http://localhost:8080/api/system)

If running correctly you should get a bunch of metrics - it should look something like this:

```javascript
{"ledger":{"processed":0,"storingPerShard":0,"storedPerShard":":str:0.0","stored":1,"storing.peak":0,"latency":{"path":0,"persist":0},"checksum":9155064537244249607,"processing":0,"faults":{"tears":0,"assists":0,"stitched":0,"failed":0},"storing":0},"agent":{"protocol":100,"name":":str:/Radix:/2700000","version":2700000},"hid":":uid:8c0976a9ed622c4c0657f57a81c38f85","memory":{"total":2058354688,"max":2058354688,"free":1863937832},"nid":":uid:8cf1ffca3da4bcb3490067ad8e9273e4","serializer":"api.local_system","commitment":":hsh:0100000000000000000000000000000000000000000000000000000000000000","global":{"stored":1,"processing":0,"storing":0},"processors":2,"clock":1,"version":100,"planck":25853760,"shards":{"anchor":-8290564194929689421,"serializer":"radix.shard.space","range":{"high":8796093022207,"low":-8796093022208,"serializer":"radix.shards.range"}},"port":30000,"messages":{"inbound":{"processed":1,"discarded":0,"pending":0,"received":10},"outbound":{"processed":10,"aborted":0,"pending":0,"sent":10}},"events":{"broadcast":75,"processed":{"synchronous":64,"asynchronous":36},"queued":36,"processing":0,"dequeued":36},"key":":byt:A6rybyzH6BO3gp6XVA8NJdqAnL1FQMeCuJ8veFgM2mqQ","timestamp":1551225600000}
```

Congratulations, you are now successfully running the Betanet Emulator!

### Managing the Betanet Emulator

Once you've got the Betanet Emulator running, you can stop or refresh \(upgrade\) the software using the following commands:

#### Stop the Betanet Emulator

```bash
docker-compose -p radixdlt -f betanet-emulator.yml down
```

#### Refresh the Betanet Emulator

```bash
docker-compose -p radixdlt -f betanet-emulator.yml pull
```

### Kitematic  <a id="kitematic"></a>

This is optional, but if you are running the Betanet Emulator on a Mac or Windows computer, you can download [Kitematic](https://kitematic.com/) to add a UI to your Docker container. If you want access to nice buttons and a live log view this is definitely for you!

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

