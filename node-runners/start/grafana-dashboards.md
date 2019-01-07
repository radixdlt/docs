# Grafana Dashboards

## Introduction

This page demonstrates how the [Prometheus Metrics](prometheus-metrics.md) can be used in Grafana Dashboards.

## Creating a Free Grafana Account

Although its easy to run your own Grafana service, it is trival to [open a free Grafana account](https://grafana.com/signup). This enables you to create your own Dashboards to monitor your nodes.

## Creating the prometheus-node Data Source

This is the Prometheus URL on your node. You need to set:

1. The **public IP** your node \([https://localhost/prometheus](https://localhost/prometheus) is not reachable from the internet\).
2. The `metrics` user's password

The rest should be like:

[![grafana-data-source](https://github.com/radixdlt/node-runner/raw/master/docs/grafana-data-source.jpg)](https://github.com/radixdlt/node-runner/blob/master/docs/grafana-data-source.jpg)

## Import the prometheus-node Dashboard

The quickest way to start is to import the [prometheus-node.json](https://github.com/radixdlt/node-runner/blob/master/docs/prometheus-node.json) Dashboard.

**NOTE**: You data source name must be `prometheus-node`.

You should see something like this:

[![grafana-dashboard](https://github.com/radixdlt/node-runner/raw/master/docs/grafana-dashboard.jpg)](https://github.com/radixdlt/node-runner/blob/master/docs/grafana-dashboard.jpg)

## Join the Radix Community

* ​[Telegram](https://t.me/radixdlt) for general chat
* ​[Discord](https://discord.gg/7Q7HSZZ) for developer chat
* ​[Reddit](https://reddit.com/r/radix) for general discussion
* ​[Forum](https://forum.radixdlt.com/) for technical discussion
* ​[Twitter](https://twitter.com/radixdlt) for announcements
* ​[Email newsletter](https://radixdlt.typeform.com/to/nyKvMV) for weekly updates
* Mail to [hello@radixdlt.com](mailto:info@radixdlt.com) for general enquiries.

## 

