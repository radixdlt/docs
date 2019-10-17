# Certificates and DNS

## Introduction

The in-browser [radixdlt-js](https://docs.radixdlt.com/radixdlt-js/) library requires a valid TLS certificate installed on the Radix nodes to work properly. As there is no way around this requirement, we are offering a simple certificate generation service by leveraging CloudFlare's infrastructure.

{% hint style="danger" %}
_**But wait, isn't this centralization?**_  
Yes, it is, and that's why we recommend using this solution only for testing purposes.
{% endhint %}

### Acquiring a certificate

For production environments, we strongly advise acquiring a valid DNS and the corresponding TLS certificate. A free service that provides TLS certificates free of charge is [Let's Encrypt](https://letsencrypt.org/). 

{% hint style="info" %}
Unfortunately, _Let's Encrypt_ ****service **is rate-limited to 50 certificates/week** so Radix cannot leverage it on behalf of our nodes**.**
{% endhint %}

## Radixnode.net service

The domain **`radixnode.net`** runs a dynamic DNS service similar to [ddns](https://github.com/pboehm/ddns). The client application for this dynamic DNS service is bundled into the node-runner docker-compose file \(as a side-car container to Radix Core\). Upon start, the client application will claim/renew the node's DNS records at `radixdnode.net`.

Additionally, CloudFlare provides valid certificates for DDoS protected end-points. This means that the associated DNS record points back to CloudFlares Edge Servers and are reverse-proxied to the Radix nodes.

### Update protocol

The conventional protocols for updating DNS records over HTTP/S usually work with a standard username/password authentication, and a reference to the hostname or subdomain selected by the user when registering the service. As these protocols are unsuitable for Radix, we designed a specific challenge/response update protocol.

The Radixnode.net update protocol limits a node to claim its own unique node hostname only \(see the [next section](certificates-and-dns.md#address-to-hostname-mapping) for details\). In order to do it, the client needs to prove to the server that it has access to the `node.key`.

### Address to hostname mapping

The specific hostname to IP mapping for the Radixnode.net service is defined as follows:

#### IPv4 addresses

* hostname begins with "a"
* the rest of the hostname label is the base36 encoded raw IPv4 address.

#### IPv6 addresses

* hostname begins with a "b"
* the rest of the hostname label is the base36 encoded raw IPv6 address.

For example:

* a node with IPv4 **`127.0.0.1`**, is entitled to claim the **`az8kflt.radixnode.net`** A record in DNS.
* a node with IPv6 address **`::1`**, is entitled to claim the **`b1.radixnode.net`** AAAA record in DNS.

{% hint style="success" %}
**Tip:** you can check the hostname entitled to your IP address using [https://api-v1.radixnode.net/myname](https://api-v1.radixnode.net/myname)
{% endhint %}

#### Base36 encoding table

| Value | Symbol |
| :--- | :--- |
| 0 | '0' |
| 1 | '1' |
| ... | ... |
| 9 | '9' |
| 10 | 'a' |
| ... | ... |
| 35 | 'z' |

