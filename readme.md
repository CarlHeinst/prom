# Automate Now - Prometheus Monitoring

<!-- TOC -->

- [Automate Now - Prometheus Monitoring](#automate-now---prometheus-monitoring)
    - [Overview](#overview)
    - [Monitor Docker Daemons](#monitor-docker-daemons)
    - [Monitor Windows Servers](#monitor-windows-servers)

<!-- /TOC -->

## Overview

This integration is designed to provide simplified monitoring for Automate Now instances.

Prometheus was used because it is light weight and relatively quick to deploy within Docker containers by using a docker-compose file.

It also has the ability to export detected alerts as webhooks which can then be consumed by IPcenter to trigger event based automations.

## Monitor Docker Daemons

This article from Docker explains in detail how to configure a Docker Engine to export it's metrics to a web source that Prometheus can read:

https://docs.docker.com/config/thirdparty/prometheus/

To summarise however in our use case the following file must be populated with the following information:

```
/etc/docker/daemon.json

{
  "metrics-addr" : "{EXTERNAL IP ADDRESS OF DOCKER SERVER}:9323",
  "experimental" : true
}
```

## Monitor Windows Servers

Install the Prometheus Windows Metrics Exposing Client:

https://github.com/martinlindhe/wmi_exporter

Clone this repo into a folder.

Before you can use the Alertmonitor component of Prometheus you must make the following edit to prometheus.yml

```
alerting:
  alertmanagers:
    - scheme: http
      #path_prefix: /
      static_configs:
        - targets: 
          - '{REPLACE ME}:9093'
```

The targets ip address must be set to the IP address of the machine that will be running the Docker containers.

```
docker-compose up
```

This work was largely inspired by this page:

https://linoxide.com/containers/setup-monitoring-docker-containers-prometheus/# prom
