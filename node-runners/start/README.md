# Node Client

## Introduction <a id="introduction"></a>

{% hint style="danger" %}
**Please note that the Node Client software is not yet available.**
{% endhint %}

This is a [Quick Start](./) guide to running a Radix DLT Node on your computer primarily aimed for non-technical users. The following steps will be covered: 

1. [Installing Docker](./#installing-docker)
2. [Creating a configuration file](./#creating-a-docker-composeyml-configuration-file)
3. [Launching your Radix DLT Node](./#launching-your-node)

## Pre-requisites <a id="pre-requisites"></a>

### Hardware <a id="hardware"></a>

Your targeted node should have **at least**:

* 2 CPU cores
* 8 GB memory
* 256 GB disk

{% hint style="warning" %}
The actual disk size requirement will grow over time as the ledger grows.
{% endhint %}

### Software <a id="software"></a>

You can run a Radix DLT node on any operative system that supports Docker and Docker Compose, including:

* Linux
* MacOS X
* Windows 10

### Forwarding incoming traffic to your Node <a id="forwarding-incoming-traffic-to-your-node"></a>

{% hint style="info" %}
You can skip this if your node is directly connected to the Internet \(has a public IP address\).
{% endhint %}

If you are behind a firewall/NAT \(typically `192.168.*`, `10.*` IP address\), then you need to forward traffic to your node including:

1. Incoming Gossip traffic on **TCP port 20000**.
2. Incoming Gossip traffic on **UDP port 20000**.
3. Incoming HTTPS traffic on **TCP port 443**.

Furthermore, you need to make sure your DHCP server is assigning a **static IP address** to your node, otherwise forwarded traffic will fail if your node’s IP address changes later on.

These are rather straight forward changes that most consumer Routers support. Please refer to the user guide of your Router for how to do this.

{% hint style="info" %}
These ports might change for _Beta_ - please check back later.
{% endhint %}

## Installing Docker <a id="installing-docker"></a>

You can download the right Docker Engine \(Community Edition\) for your system here: [https://hub.docker.com/search/?type=edition&offering=community](https://hub.docker.com/search/?type=edition&offering=community)

For a detailed Docker setup guide, please refer to the official [installation documentation](https://docs.docker.com/install/) for [Docker CE](https://www.docker.com/products/docker-engine).

### Installing Docker Compose \(Linux only\)

If you are running Linux, after you completed the Docker setup you need to install [Docker Compose](https://docs.docker.com/compose/install/). See the official Docker Composite [installation guide](https://docs.docker.com/compose/install/) for download links and additional information.

{% hint style="success" %}
**Tip:** Docker Compose is bundled with Docker CE for the Mac and Windows versions.
{% endhint %}

## Creating a configuration file <a id="creating-a-docker-composeyml-configuration-file"></a>

The Radix DLT software stack is composed of a set of specialised docker images, with different roles. The minimal \(basic\) radixdlt system contains the following components \(docker images\):

1. `radixdlt/radixdlt-core:alpha2`
2. `radixdlt/radixdlt-nginx:alpha2`
3. `radixdlt/nns-client:alpha2`

Your `docker-compose.yml` determines the software components you will run. In particular the following is specified:

* One or more Docker images to download and start
* The configuration \(see [environment variables](./#node-configuration-options)\) for each Docker image
* Persistent data volumes - that survive restarts and upgrades.

Start with this docker compose file:

{% code-tabs %}
{% code-tabs-item title="basic-node.yml" %}
```yaml
version: '2.2'
services:
  core:
    image: radixdlt/radixdlt-core:alpha2
    init: true
    restart: unless-stopped
    ports:
      - "20000:20000/tcp"
      - "20000:20000/udp"
    environment:
      CORE_GOSSIP_PORT: 20000
      CORE_NETWORK_SEEDS: "13.67.90.17,13.67.77.243,13.250.38.73,13.250.122.64,13.229.71.105,18.191.172.155,168.61.34.193,168.62.51.26,18.191.232.20,13.58.101.137,40.91.210.108,104.45.18.105,52.56.134.133,35.176.114.13,35.176.203.64,13.66.213.50,13.66.168.246,34.215.16.37,18.237.250.3,52.11.121.107,191.239.255.182,191.239.245.47,52.67.254.26,52.67.23.104"
      CORE_NETWORK_DISCOVERY_URLS: "https://alphanet2.radixdlt.com/node-finder"
      CORE_PARTITION_FRAGMENTS: 1
      # Read the docs before changing this.
      CORE_SECURE_RANDOM_SOURCE: /dev/urandom      
      CORE_UNIVERSE: "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"
    volumes:
      - "core_ledger:/opt/radixdlt/RADIXDB"
      - "core_config:/opt/radixdlt/etc"
    logging:
      options:
        max-size: "10m"
        max-file: "30"
    cap_add:
      - NET_ADMIN
  nginx:
    image: radixdlt/radixdlt-nginx:alpha2
    restart: unless-stopped
    ports:
      - "443:443"
    environment:
      WIPE_ADMIN_PASSWORD: "no"
    volumes:
      - "nginx_secrets:/etc/nginx/secrets"
    logging:
      options:
        max-size: "1m"
        max-file: "30"
  nns:
    image: radixdlt/nns-client:alpha2
    restart: unless-stopped
    logging:
      options:
        max-size: "1m"
        max-file: "30"
volumes:
  core_ledger:
  core_config:
  nginx_secrets:

```
{% endcode-tabs-item %}
{% endcode-tabs %}

1. Create a directory on your computer for storing docker compose files \(e.g. `radixdlt`\).
2. Use your favorite text editor to create the `basic-node.yml`.
3. Copy-and-paste the content above.

{% hint style="success" %}
**Tip:** you can also download the latest version of the file here: [`basic-node.yml`](https://github.com/radixdlt/node-runner/blob/master/docs/basic-node.yml).
{% endhint %}

## Launching your Node <a id="launching-your-node"></a>

Open up Terminal \(Mac/Linux\) or CMD on Windows. Navigate to the directory that you put your `docker-composer.yml` file in. Launch the Docker containers with:

```bash
cd ~/radixdlt
docker-compose -p radixdlt -f basic-node.yml up -d
```

If successful, it should pull down and look something like this when completed:

```text
Pulling core (radixdlt.azurecr.io/radixdlt/radixdlt-core:latest-alpha)...
latest-alpha: Pulling from radixdlt/radixdlt-core
...
Digest: sha256:d7f31770d1060d20ffd8f21365158937e893e4d3ce5ccdc089d1d11bbf26d4e0
Status: Downloaded newer image for radixdlt.azurecr.io/radixdlt/radixdlt-core:latest-alpha
Pulling nginx (radixdlt/radixdlt-nginx:latest-alpha)...
latest-alpha: Pulling from radixdlt/radixdlt-nginx
...
Digest: sha256:0f38c6706e2a2e6ff20e0167d266998dc4d2813e1b12ede644cfd97c9127161c
Status: Downloaded newer image for radixdlt/radixdlt-nginx:latest-alpha
Creating radixdlt_nginx_1 ... done
Creating radixdlt_core_1  ... done
```

Check that you have 3 \(`radixdlt_nginx_1`, `nns_client_1`, and `radixdlt_core_1`\) Docker containers with:

```bash
docker ps
```

Make note and write down the `admin` password - its written in the `radixdlt_nginx_1` container logs the **first time** it starts:

```bash
docker logs radixdlt_nginx_1
```

This password is used for accessing administrative APIs on your node. If you forget, you can re-generate this at any time by setting the `WIPE_ADMIN_PASSWORD` environment variable.

You can also check if the Node is up and running at [https://localhost/api/system](https://localhost/api/system)

{% hint style="warning" %}
**Note**: Since it is a self-signed certificate browsers are expected to warn you that this link is unsafe - you can disregard this for Alpha/Beta.
{% endhint %}

If running correctly you should get a bunch of metrics - it should look something like this:

```javascript
{"ledger":{"processed":0,"latency":{"path":0,"persist":0},"stored":3,"checksum":2193713224449319881,"processing":0,"faults":{"tears":0,"stitched":0},"storing":0},"agent":{"protocol":100,"name":"/Radix:/2300000","version":2300000},"hid":{"serializer":"EUID","value":"13115213306523712699347341883"},"period":0,"memory":{"total":122683392,"max":466092032,"free":61252608},"commitment":{"serializer":"HASH","value":"0000000000000000000000000000000000000000000000000000000000000000"},"serializer":-1833998801,"clock":0,"processors":4,"version":100,"shards":{"high":9223372036854775807,"low":-9223372036854775808},"port":20000,"messages":{"processed":0,"processing":0},"events":{"processed":27,"processing":0},"key":{"serializer":"BASE64","value":"AtadBccJwmoeY70gWLM0hQyTJtbhROrupp9A4/DHzXMa"}}
```

After around a minute or so, your new Node should also have found some Peers - to check it’s peer grouping, look here: [https://localhost/api/network/peers](https://localhost/api/network/peers)

If it is working correctly, you should have around a full browser page of peer information that looks something like this:

```javascript
{"serializer":"BASE64","value":"AtvDaWQgPRftFxpybWD/1Yyt3w5UPI510bp6+ruQ3+Sf"}},"host":
{"port":20000,"ip":"13.66.168.246"},"serializer":2451810,"version":100}, 
{"system":{"shards":{"high":6917529027641081855,"low":4611686018427387904},"agent":
{"protocol":100,"name":"/Radix:/2270000","version":2270000},"period":152913929,"port":20000,"commitment":
{"serializer":"HASH","value":"0000000000000000000000000000000000000000000000000000000000000000"},"serializer":-1833998801,"clock":114,"version":100,"key":
```

Congratulations, you are now successfully running a Radix Node!

### Kitematic <a id="kitematic"></a>

This is optional, but if you are running your Node on a Mac or Windows computer you can download [Kitematic](https://kitematic.com/) to add a UI to your Docker container: If you want access to nice buttons and a live log view; this is definitely for you!

### Node configuration options <a id="node-configuration-options"></a>

Changing the configuration below in your docker compose file requires that your re-run docker compose:

```bash
docker-compose -p radixdlt -f basic-node.yml up -d
```

#### WIPE\_ADMIN\_PASSWORD <a id="wipe_admin_password"></a>

Setting this to `yes` and restarting the `nginx` service will wipe and regenerate the `admin` user’s password.

#### WIPE\_LEDGER <a id="wipe_ledger"></a>

Setting this to `yes` and restarting the `core` service will wipe the local ledger and re-sync it from other nodes in the Radix DLT network.

#### WIPE\_NODE\_KEY <a id="wipe_node_key"></a>

Setting this to `yes` and restarting the `core` service will wipe the `node.key` file, which is your RadixDLT identity on the network. Hence, you will get a new identity and probably end up in a different shard.

#### CORE\_GOSSIP\_PORT <a id="core_gossip_port"></a>

This is `20000` for the Alpha network and needs to match the port encoded in the `CORE_UNIVERSE` string.

#### CORE\_NETWORK\_SEEDS <a id="core_network_seeds"></a>

Concrete IP address for discovering other nodes on the Radix DLT network.

Either `CORE_NETWORK_SEEDS` or `CORE_NETWORK_DISCOVERY_URLS` or both need to be set.

#### CORE\_NETWORK\_DISCOVERY\_URLS <a id="core_network_discovery_urls"></a>

The URL to a simple web service, which returns an IP address to a random node on the Radix DLT network. This IP address will be used for discovering other nodes on the network.

Either `CORE_NETWORK_DISCOVERY_URLS` or `CORE_NETWORK_SEEDS` or both need to be set.

#### CORE\_PARTITION\_FRAGMENTS <a id="core_partition_fragments"></a>

Number of shards that the target network is partition in. For the Alpha network is partitioned into `1`shards.

#### CORE\_UNIVERSE <a id="core_universe"></a>

Universe identity and properties \(such as gossip port\). This string separates two Radix DLT networks from each other.

### CORE\_SECURE\_RANDOM\_SOURCE <a id="core_secure_random_source"></a>

Secure random number device used by the JVM. The default device is [/dev/urandom](https://linux.die.net/man/4/urandom).
However, power users might want to switch to `/dev/random`, in which case a decent a entropy generator side-car container
is needed (e.g [haveged](https://linux.die.net/man/8/haveged)).
**NOTE**: Switching to `/dev/random` without an decent entropy generator might cause unexpected periodical stalls (hanging).


## Next: monitor your Radix Node

There are a bunch of frameworks for managing metrics. In the [next guide](), we extend the `basic-node.yml` to include the [Prometheus](https://prometheus.io/) service. Prometheus periodically collects metrics system, and application specific metrics and stores these in its local datastore.

## Join the Radix Community

* ​[Telegram](https://t.me/radix_dlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [hello@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.

