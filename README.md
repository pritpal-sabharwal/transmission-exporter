# Transmission Exporter for Prometheus [![Build Status](https://cloud.drone.io/api/badges/metalmatze/transmission-exporter/status.svg)](https://cloud.drone.io/metalmatze/transmission-exporter)

[![Docker Pulls](https://img.shields.io/docker/pulls/metalmatze/transmission-exporter.svg?maxAge=604800)](https://hub.docker.com/r/metalmatze/transmission-exporter)
[![Go Report Card](https://goreportcard.com/badge/github.com/metalmatze/transmission-exporter)](https://goreportcard.com/report/github.com/metalmatze/transmission-exporter)

Prometheus exporter for [Transmission](https://transmissionbt.com/) metrics, written in Go.

# LOOKING FOR MAINTAINERS
I don't use this exporter anymore and I'd be happy if others would want to take over and maintain it in the future!  
Write me a DM via [Twitter](https://twitter.com/metalmatze)!

### Installation

    $ go get github.com/metalmatze/transmission-exporter

### Configuration

ENV Variable | Description
|----------|-----|
| WEB_PATH | Path for metrics, default: `/metrics` |
| WEB_ADDR | Address for this exporter to run, default: `:19091` |
| WEB_CONFIG_FILE | Configuration file to protect exporter with TLS and/or basic auth, no default |
| TRANSMISSION_ADDR | Transmission address to connect with, default: `http://localhost:9091` |
| TRANSMISSION_USERNAME | Transmission username, no default |
| TRANSMISSION_PASSWORD | Transmission password, no default |
| LOG_FORMAT | Specify log output format. Options are 'text' (default) or 'json' |

### Docker

    docker pull metalmatze/transmission-exporter
    docker run -d -p 19091:19091 metalmatze/transmission-exporter

### Kubernetes (Prometheus)

A sample kubernetes manifest is available in [example/kubernetes](https://github.com/metalmatze/transmission-exporter/blob/master/examples/kubernetes/docker-compose.yml)

Please run: `kubectl apply -f examples/kubernetes/transmission.yml`

You should:
* Attach the config and downloads volume
* Configure the password for the exporter

Your prometheus instance will start scraping the metrics automatically. (if configured with annotation based discovery). [more info](https://www.weave.works/docs/cloud/latest/tasks/monitor/configuration-k8s/)

### Docker Compose

Example `docker-compose.yml` with Transmission also running in docker.

    transmission:
      image: linuxserver/transmission
      restart: always
      ports:
        - "127.0.0.1:9091:9091"
        - "51413:51413"
        - "51413:51413/udp"
    transmission-exporter:
      image: metalmatze/transmission-exporter
      restart: always
      links:
        - transmission
      ports:
        - "127.0.0.1:19091:19091"
      environment:
        TRANSMISSION_ADDR: http://transmission:9091

### TLS and Basic Authentication

This exporter uses the Prometheus exporter-toolkit to provide TLS and basic authentication support.

See the [exporter-toolkit web configuration](https://github.com/prometheus/exporter-toolkit/blob/master/docs/web-configuration.md) for more details.

### Development

    make

For development we encourage you to use `make install` instead, it's faster.

Now simply copy the `.env.example` to `.env`, like `cp .env.example .env` and set your preferences.
Now you're good to go.

### Original authors of the Transmission package  
Tobias Blom (https://github.com/tubbebubbe/transmission)  
Long Nguyen (https://github.com/longnguyen11288/go-transmission)
