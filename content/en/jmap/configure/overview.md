---
title: "Overview"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "overview"
weight: 201
toc: true
---

## Introduction

Stalwart JMAP can be configured in three different ways, with a config file in YAML format, using command
line arguments and also through environment variables. It is also possible to combine all three methods.

## Configuration File

To read the configuration settings from a YAML file, start Stalwart JMAP with the ``--config`` parameter
which takes as argument the path to the configuration file in YAML format, for example:

```
$ /usr/local/stalwart-jmap/bin/stalwart-jmap --config=/usr/local/stalwart-jmap/etc/config.yml
```

Within the YAML configuration file, parameters are specified using the format ``<parameter>: <value>``.
For example, to set the logging level to ``info`` in the config file:

```
log-level: info
```

## Command Line

In cases where not many default parameters need to be changed, it might be convenient to configure
Stalwart JMAP directly from the command line. Each argument is passed using the format ``--<parameter>=<value>``.
For example, to start Stalwart JMAP with the default settings using command line arguments:

```
$ /usr/local/stalwart-jmap/bin/stalwart-jmap --jmap-url=https://jmap.example.org:8080 \
                         --jmap-cert-path=/usr/local/stalwart-jmap/etc/certs/jmap.crt \
                         --jmap-key-path=/usr/local/stalwart-jmap/etc/private/jmap.key \
                         --encryption-key=HERE_GOES_THE_KEY
```

You may also override one or multiple parameters in the configuration file from the command line. For example,
to temporarily change the logging level to ``debug``:

```
$ /usr/local/stalwart-jmap/bin/stalwart-jmap --config=/usr/local/stalwart-jmap/etc/config.yml \
                         --log-level=debug 
```

## Environment Variables

Environment variables are particularly useful when starting Stalwart JMAP from a Docker container. Parameters specified
as environment variables are expected in uppercase and hyphenated using underscores ``_`` rather than dashes ``-``.
For example, to start Stalwart JMAP with the default settings using environment variables:

```
$ export JMAP_URL="https://jmap.example.org:8080" \
         JMAP_CERT_PATH="/usr/local/stalwart-jmap/etc/certs/jmap.crt" \
         JMAP_KEY_PATH="/usr/local/stalwart-jmap/etc/private/jmap.key" \
         ENCRIPTION_KEY="HERE_GOES_THE_KEY"
$ /usr/local/stalwart-jmap/bin/stalwart-jmap
```

You may also override both configuration file and command line arguments using environment variables:

```
$ LOG_LEVEL=debug /usr/local/stalwart-jmap/bin/stalwart-jmap --config=/usr/local/stalwart-jmap/etc/config.yml
```
