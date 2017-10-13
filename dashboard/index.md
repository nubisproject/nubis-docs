# Nubis Dashboard

[![Version](https://img.shields.io/github/release/nubisproject/nubis-base.svg?maxAge=2592000)](https://github.com/nubisproject/nubis-base/releases)
[![Build Status](https://img.shields.io/travis/nubisproject/nubis-base/master.svg?maxAge=2592000)](https://travis-ci.org/nubisproject/nubis-base)
[![Issues](https://img.shields.io/github/issues/nubisproject/nubis-base.svg?maxAge=2592000)](https://github.com/nubisproject/nubis-base/issues)

Welcome to the Nubis Dashboard. Here you will find links to all of the tools
available in this account.

We have also included some sample, cheat sheet style help for you along with
links to documentation about each tool.

If you have any questions, please reach out to the Nubis team on slack or irc
in #nubis-users.

* [Key-Value](#key-value)
* [Monitoring](#monitoring)
* [Alerting](#alerting)
* [Graphs](#graphs)
* [Logs](#logs)
* [Pipeline](#pipeline)

## Key-Value

The key-value store is where run-time configuration is stored. We are using
[Consul](https://www.consul.io/) as both the key-value store as well as for
service discovery.

**TODO:** Add me some helpfull cheat-sheet commands.

You can find the official documentation [here](https://www.consul.io/docs/index.html).

## Monitoring

We are using [Prometheus](https://prometheus.io/) as the monitoring service in
this account.

**TODO:** Add me some helpfull cheat-sheet commands.

You can find the official documentation [here](https://prometheus.io/docs/introduction/overview/).

## Alerting

The alert manager we are using is the native [Prometheus Alert Manager](https://prometheus.io/docs/alerting/alertmanager/).

**TODO:** Add me some helpful cheat-sheet commands.

You can find the official documentation [here](https://prometheus.io/docs/alerting/alertmanager/).

## Graphs

[Grafana](https://prometheus.io/docs/visualization/grafana/) is the really nice
tool that is available to you to make sense of all of your log and monitoring
data.

**TODO:** Add me some helpful cheat-sheet commands.

You can find the official documentation [here](https://prometheus.io/docs/visualization/grafana/).

## Logs

We use [Fluentd](http://www.fluentd.org/) to aggregate logs from all nodes and
services. These logs are then inserted into an [Elasticsearch](https://www.elastic.co/)
cluster. These are presented through a Kibana interface.

**TODO:** Add me some helpful cheat-sheet commands.

You can find the official Kibana documentation [here](https://www.elastic.co/guide/en/kibana/current/index.html).

You can find the official Elasticsearch documentation [here](https://www.elastic.co/guide/index.html).

You can find the official Fluentd documentation [here](http://docs.fluentd.org/v0.12/articles/quickstart).

## Pipeline

[Jenkins](https://jenkins.io/) is the Continuous Integration (CI) system
deployed in this account. This is where you go to promote your staging builds
into production.

**TODO:** Add me some helpful cheat-sheet commands.

You can find the official documentation [here](https://jenkins.io/doc/).
