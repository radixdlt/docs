# Betanet Emulator

## Introduction <a id="introduction"></a>

This is a quick start guide aimed for non-technical users, to run a Radix DLT Betanet Emulator on your computer. The following steps are covered:

1. [Installing Docker](/#installing-docker)
2. [Creating a configuration file](/#creating-a-docker-composeyml-configuration-file)
3. [Launching the Radix DLT Betanet Emulator](/#launching-emulator)

## Pre-requisites <a id="pre-requisites"></a>

### Hardware <a id="hardware"></a>

Your host computer should have **at least**:

* 2 CPU cores
* 8 GB memory
* 256 GB disk

{% hint style="info" %}
**Note:** the actual disk size requirement will grow over time as the ledger grows.
{% endhint %}

### Software <a id="software"></a>

You can run the Radix DLT Betanet Emulator on any operating system that supports Docker and Docker Compose, including:

* Linux
* MacOS X
* Windows 10

## Installing Docker <a id="installing-docker"></a>

You can download the right Docker Engine \(Community Edition\) for your system here: [https://hub.docker.com/search/?type=edition&offering=community](https://hub.docker.com/search/?type=edition&offering=community)

For a detailed Docker setup guide, please refer to the official [installation documentation](https://docs.docker.com/install/) for [Docker CE](https://www.docker.com/products/docker-engine).

### Installing Docker Compose \(Linux only\)

If you are running Linux, after you completed the Docker setup you need to install [Docker Compose](https://docs.docker.com/compose/install/). See the official Docker Composite [installation guide](https://docs.docker.com/compose/install/) for download links and additional information.

{% hint style="success" %}
**Tip:** Docker Compose is bundled with Docker CE for the Mac and Windows versions.
{% endhint %}

## Creating a configuration file <a id="creating-a-docker-composeyml-configuration-file"></a>

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
      CORE_UNIVERSE: v2djcmVhdG9yWCIBAgzfn1Z35c749BQlEGUonphiGt2BIntgL/Zq9py9rk7ha2Rlc2NyaXB0aW9ud1RoZSBSYWRpeCB0ZXN0IFVuaXZlcnNlZ2dlbmVzaXOBv2htZXRhRGF0Yb9pdGltZXN0YW1wbTE1NTEyMjU2MDAwMDD/bnBhcnRpY2xlR3JvdXBzg79pcGFydGljbGVzgb9ocGFydGljbGW/ZWJ5dGVzVwFSYWRpeC4uLiBqdXN0IGltYWdpbmUhbGRlc3RpbmF0aW9uc4FRAiLoX23FEsoqqrhxPrIKuQJkZnJvbVgnBAECDN+fVnflzvj0FCUQZSiemGIa3YEie2Av9mr2nL2uTuEDgXBNZW5vbmNlGwAAKdFQksTEanNlcmlhbGl6ZXJ3cmFkaXgucGFydGljbGVzLm1lc3NhZ2VidG9YJwQBAgzfn1Z35c749BQlEGUonphiGt2BIntgL/Zq9py9rk7hA4FwTWd2ZXJzaW9uGGT/anNlcmlhbGl6ZXJzcmFkaXguc3B1bl9wYXJ0aWNsZWRzcGluAWd2ZXJzaW9uGGT/anNlcmlhbGl6ZXJ0cmFkaXgucGFydGljbGVfZ3JvdXBndmVyc2lvbhhk/79pcGFydGljbGVzg79ocGFydGljbGW/bGRlc3RpbmF0aW9uc4FRAiLoX23FEsoqqrhxPrIKuQJlbm9uY2UAY3JyaVg5Bi85ZWNqTU5DRkRTYkxaeFZwZmJGd0ZUTFd1TDdTSDNRNDl1ekdycEszYlVjemU2Q0p0RHIvWFJEanNlcmlhbGl6ZXJzcmFkaXgucGFydGljbGVzLnJyaWd2ZXJzaW9uGGT/anNlcmlhbGl6ZXJzcmFkaXguc3B1bl9wYXJ0aWNsZWRzcGluIGd2ZXJzaW9uGGT/v2hwYXJ0aWNsZb9nYWRkcmVzc1gnBAECDN+fVnflzvj0FCUQZSiemGIa3YEie2Av9mr2nL2uTuEDgXBNa2Rlc2NyaXB0aW9uc1JhZGl4IE5hdGl2ZSBUb2tlbnNsZGVzdGluYXRpb25zgVECIuhfbcUSyiqquHE+sgq5AmtncmFudWxhcml0eVghBQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABZ2ljb25Vcmx4NGh0dHBzOi8vYXNzZXRzLnJhZGl4ZGx0LmNvbS9pY29ucy9pY29uLXhyZC0zMngzMi5wbmdkbmFtZWRSYWRza3Blcm1pc3Npb25zv2RidXJuZG5vbmVkbWludHN0b2tlbl9jcmVhdGlvbl9vbmx5/2pzZXJpYWxpemVyeCByYWRpeC5wYXJ0aWNsZXMudG9rZW5fZGVmaW5pdGlvbmZzeW1ib2xjWFJEZ3ZlcnNpb24YZP9qc2VyaWFsaXplcnNyYWRpeC5zcHVuX3BhcnRpY2xlZHNwaW4BZ3ZlcnNpb24YZP+/aHBhcnRpY2xlv2ZhbW91bnRYIQX//////////////////////////////////////////2xkZXN0aW5hdGlvbnOBUQIi6F9txRLKKqq4cT6yCrkCa2dyYW51bGFyaXR5WCEFAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAFlbm9uY2UbAAAp0VCTGUtrcGVybWlzc2lvbnO/ZGJ1cm5kbm9uZWRtaW50c3Rva2VuX2NyZWF0aW9uX29ubHn/anNlcmlhbGl6ZXJ4InJhZGl4LnBhcnRpY2xlcy51bmFsbG9jYXRlZF90b2tlbnN4GHRva2VuRGVmaW5pdGlvblJlZmVyZW5jZVg5Bi85ZWNqTU5DRkRTYkxaeFZwZmJGd0ZUTFd1TDdTSDNRNDl1ekdycEszYlVjemU2Q0p0RHIvWFJEZ3ZlcnNpb24YZP9qc2VyaWFsaXplcnNyYWRpeC5zcHVuX3BhcnRpY2xlZHNwaW4BZ3ZlcnNpb24YZP9qc2VyaWFsaXplcnRyYWRpeC5wYXJ0aWNsZV9ncm91cGd2ZXJzaW9uGGT/v2lwYXJ0aWNsZXODv2hwYXJ0aWNsZb9mYW1vdW50WCEF//////////////////////////////////////////9sZGVzdGluYXRpb25zgVECIuhfbcUSyiqquHE+sgq5AmtncmFudWxhcml0eVghBQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABZW5vbmNlGwAAKdFQkxlLa3Blcm1pc3Npb25zv2RidXJuZG5vbmVkbWludHN0b2tlbl9jcmVhdGlvbl9vbmx5/2pzZXJpYWxpemVyeCJyYWRpeC5wYXJ0aWNsZXMudW5hbGxvY2F0ZWRfdG9rZW5zeBh0b2tlbkRlZmluaXRpb25SZWZlcmVuY2VYOQYvOWVjak1OQ0ZEU2JMWnhWcGZiRndGVExXdUw3U0gzUTQ5dXpHcnBLM2JVY3plNkNKdERyL1hSRGd2ZXJzaW9uGGT/anNlcmlhbGl6ZXJzcmFkaXguc3B1bl9wYXJ0aWNsZWRzcGluIGd2ZXJzaW9uGGT/v2hwYXJ0aWNsZb9mYW1vdW50WCEF///////////////////////////8xNHDYC9/wxf///9sZGVzdGluYXRpb25zgVECIuhfbcUSyiqquHE+sgq5AmtncmFudWxhcml0eVghBQAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABZW5vbmNlGwAAKdFQk1fna3Blcm1pc3Npb25zv2RidXJuZG5vbmVkbWludHN0b2tlbl9jcmVhdGlvbl9vbmx5/2pzZXJpYWxpemVyeCJyYWRpeC5wYXJ0aWNsZXMudW5hbGxvY2F0ZWRfdG9rZW5zeBh0b2tlbkRlZmluaXRpb25SZWZlcmVuY2VYOQYvOWVjak1OQ0ZEU2JMWnhWcGZiRndGVExXdUw3U0gzUTQ5dXpHcnBLM2JVY3plNkNKdERyL1hSRGd2ZXJzaW9uGGT/anNlcmlhbGl6ZXJzcmFkaXguc3B1bl9wYXJ0aWNsZWRzcGluAWd2ZXJzaW9uGGT/v2hwYXJ0aWNsZb9nYWRkcmVzc1gnBAECDN+fVnflzvj0FCUQZSiemGIa3YEie2Av9mr2nL2uTuEDgXBNZmFtb3VudFghBQAAAAAAAAAAAAAAAAAAAAAAAAAAAzsuPJ/QgDzoAAAAbGRlc3RpbmF0aW9uc4FRAiLoX23FEsoqqrhxPrIKuQJrZ3JhbnVsYXJpdHlYIQUAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAWVub25jZRsAACnRUJNIyWtwZXJtaXNzaW9uc79kYnVybmRub25lZG1pbnRzdG9rZW5fY3JlYXRpb25fb25sef9mcGxhbmNrGgGKf0Bqc2VyaWFsaXplcngkcmFkaXgucGFydGljbGVzLnRyYW5zZmVycmFibGVfdG9rZW5zeBh0b2tlbkRlZmluaXRpb25SZWZlcmVuY2VYOQYvOWVjak1OQ0ZEU2JMWnhWcGZiRndGVExXdUw3U0gzUTQ5dXpHcnBLM2JVY3plNkNKdERyL1hSRGd2ZXJzaW9uGGT/anNlcmlhbGl6ZXJzcmFkaXguc3B1bl9wYXJ0aWNsZWRzcGluAWd2ZXJzaW9uGGT/anNlcmlhbGl6ZXJ0cmFkaXgucGFydGljbGVfZ3JvdXBndmVyc2lvbhhk/2pzZXJpYWxpemVyanJhZGl4LmF0b21qc2lnbmF0dXJlc794IDIyZTg1ZjZkYzUxMmNhMmFhYWI4NzEzZWIyMGFiOTAyv2FyWCEBum4RNOnFKq+zgjHt1MYtr+mn/y5/bhxd2nNR29fHu0lhc1ghAQFH3brv3i+/UdbQpJDKfiIq4fAp+NksSjHER5OmkOabanNlcmlhbGl6ZXJ2Y3J5cHRvLmVjZHNhX3NpZ25hdHVyZWd2ZXJzaW9uGGT//210ZW1wb3JhbFByb29mv2NhaWRYIQj531pldLy+9t+aKtgPkOebBiPtPbLjGvsi6F9txRLKKmpzZXJpYWxpemVydHRlbXBvLnRlbXBvcmFsX3Byb29mZ3ZlcnNpb24YZGh2ZXJ0aWNlc4G/ZWNsb2NrAWpjb21taXRtZW50WCEDAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAABlb3duZXJYIgEDw/b5toR/c5zhCCMc0bXPrACT9VQUBFF8zzep3GbC1BpocHJldmlvdXNRAgAAAAAAAAAAAAAAAAAAAABmcmNsb2NrGwAAAWksQDgAanNlcmlhbGl6ZXJ1dGVtcG8udGVtcG9yYWxfdmVydGV4aXNpZ25hdHVyZb9hclghAertuRosK604aNrXCqvHLtX+4Lpi1z4IXLuMHkJ9r3fHYXNYIQF2yCsbEeNZDyULZ51RJ9gvFChZQBrnjbxxSHUX2Jae9WpzZXJpYWxpemVydmNyeXB0by5lY2RzYV9zaWduYXR1cmVndmVyc2lvbhhk/2p0aW1lc3RhbXBzv2dkZWZhdWx0GwAAAWksQDgA/2d2ZXJzaW9uGGT//2d2ZXJzaW9uGGT/ZW1hZ2ljGg/mAAFkbmFtZW1SYWRpeCBUZXN0bmV0ZnBsYW5jaxnqYGRwb3J0GU4ganNlcmlhbGl6ZXJucmFkaXgudW5pdmVyc2Vrc2lnbmF0dXJlLnJYIQHrj6Fi074qwVweJCv9bW/O6jtXMgAeq1ucaR6yHXAFdmtzaWduYXR1cmUuc1ghAULsLXApQPGFcbauzDGm4Crur7SRbYpLMB9ZF/NclkiAaXRpbWVzdGFtcBsAAAFpLEA4AGR0eXBlAWd2ZXJzaW9uGGT/
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
      FAUCET_TOKEN_RRI: /9ecjMNCFDSbLZxVpfbFwFTLWuL7SH3Q49uzGrpK3bUcze6CJtDr/XRD
      RADIX_IDENTITY_UNENCRYPTED_KEY: XfodcnhdYtZj1ohBISa6EJwPK1SGrJDVXw1VobuVNLo=
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

## Launching the Betanet Emulator <a id="launching-emulator"></a>

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

### Kitematic <a id="kitematic"></a>

This is optional, but if you are running the Betanet Emulator on a Mac or Windows computer, you can download [Kitematic](https://kitematic.com/) to add a UI to your Docker container. If you want access to nice buttons and a live log view this is definitely for you!

## Join the Radix Community

* ​[Telegram](https://t.me/radix_dlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [info@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.

