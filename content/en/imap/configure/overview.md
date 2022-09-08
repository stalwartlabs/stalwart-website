---
title: "Overview"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  imap:
    parent: "configure"
    identifier: "overview"
weight: 100
toc: true
---

## Introduction

Stalwart IMAP can be configured in three different ways, with a config file in YAML format, using command
line arguments and also through environment variables. It is also possible to combine all three methods.

## Configuration File

To read the configuration settings from a YAML file, start Stalwart IMAP with the ``--config`` parameter
which takes as argument the path to the configuration file in YAML format, for example:

```
$ /usr/bin/stalwart-imap --config=/etc/stalwart-imap/config.yml
```

Within the YAML configuration file, parameters are specified using the format ``<parameter>: <value>``.
For example, to set the logging level to ``info`` in the config file:

```
log-level: info
```

## Command Line

In cases where not many default parameters need to be changed, it might be convenient to configure
Stalwart IMAP directly from the command line. Each argument is passed using the format ``--<parameter>=<value>``.
For example, to start Stalwart IMAP with the default settings using command line arguments:

```
$ /usr/bin/stalwart-imap --jmap-url=https://jmap.example.org:8080 \
                         --cert-path=/etc/stalwart-imap/certs/imap.crt \
                         --key-path=/etc/stalwart-imap/private/imap.key 
```

You may also override one or multiple parameters in the configuration file from the command line. For example,
to temporarily change the logging level to ``debug``:

```
$ /usr/bin/stalwart-imap --config=/etc/stalwart-imap/config.yml \
                         --log-level=debug 
```

## Environment Variables

Environment variables are particularly useful when starting Stalwart IMAP from a Docker container. Parameters specified
as environment variables are expected in uppercase and hyphenated using underscores ``_`` rather than dashes ``-``.
For example, to start Stalwart IMAP with the default settings using environment variables:

```
$ export JMAP_URL="https://jmap.example.org:8080" \
         CERT_PATH="/etc/stalwart-imap/certs/imap.crt" \
         KEY_PATH="/etc/stalwart-imap/private/imap.key"
$ /usr/bin/stalwart-imap
```

You may also override both configuration file and command line arguments using environment variables:

```
$ LOG_LEVEL=debug /usr/bin/stalwart-imap --config=/etc/stalwart-imap/config.yml
```
