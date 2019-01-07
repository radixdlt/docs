# Prometheus Metrics

## Introduction

By now you should be running your _Basic Node_ as described in the [QuickStart](./) page.

This page focuses on RadixDLT node monitoring.

There are a bunch of frameworks for managing metric. In this guide, we extend the basic-node.yml to include the [Prometheus](https://prometheus.io/) service. Prometheus periodically collects metrics system, and application specific metrics and stores these in its local datastore.

## Extending the Basic Node

If you are in a hurry you can just download and run [prometheus-node.yml](https://github.com/radixdlt/node-runner/blob/master/docs/prometheus-node.yml).

Below are the additions to `basic-node.yml` for adding Prometheus support to your node.

Start by copying `basic-node.yml` and name it `prometheus-node.yml`.

### Adding the WIPE\_METRICS\_PASSWORD Environment Variable

Add the environment variable in question tothe `nginx` service configuration in `prometheus-node.yml`

Setting it to `yes` and restarting the `nginx` service will wipe and regenerate the `metrics` user's password.

### Including the Exporter Service

This small python project collects docker container metrics like \(CPU, RAM, I/O\) usage as well as application specific metrics from the `core` and `nginx` services.

```text
  exporter:
    image: radixdlt/radixdlt-exporter:latest
    restart: unless-stopped
    volumes:
      - "/sys:/sys:ro"    
    environment:
      RADIXDLT_CORE_BASE_URL: http://core:8080
      RADIXDLT_NGINX_BASE_URL: https://nginx
    logging:
      options:
        max-size: "1m"
        max-file: "10"
```

This service is exposed by the `nginx` service as: [https://localhost/metrics](https://localhost/metrics) You should find a bunch of Prometheus scrapable metrics like:

```text
# HELP process_virtual_memory_bytes Virtual memory size in bytes.
# TYPE process_virtual_memory_bytes gauge
process_virtual_memory_bytes 107610112.0
# HELP process_resident_memory_bytes Resident memory size in bytes.
# TYPE process_resident_memory_bytes gauge
process_resident_memory_bytes 29077504.0
...
```

**NOTE**: This service needs read-only access to `/sys` for collectiong the other container's metrics.

### Including the Prometheus Service

Prometheus scrapes the [https://localhost/metrics](https://localhost/metrics) URL \(exporter service\) periodically \(**15 second** interval\). The `PROMETHEUS_TSDB_RETENTION` environment variable determines the data retention time.

```text
  prometheus:
    image: radixdlt/radixdlt-prometheus:latest
    restart: unless-stopped
    environment:
      PROMETHEUS_TSDB_RETENTION: "10d"
    volumes:
      - "prometheus_tsdb:/prometheus"
    logging:
      options:
        max-size: "1m"
        max-file: "30"
```

This service is reachable through the [https://localhost/prometheus](https://localhost/prometheus) URL. Because queries can be heavy \(thus used for attacking your node\), we protect it with the `metrics` user/password, which you can get from the `nginx` service logs.

The Prometheus UI is located at [https://localhost/prometheus/graph](https://localhost/prometheus/graph) 

## Next: [Build Dashboards](grafana-dashboards.md)

In the [next section](grafana-dashboards.md) you will learn how to load your prometheus metrics into [Grafana](https://grafana.com) Dashboards for easy node monitoring.

## Join the Radix Community

* ​[Telegram](https://t.me/radixdlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [hello@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.

## 



