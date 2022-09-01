---
title: "JMAP Settings"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "jmap-proto"
weight: 200
toc: true
---

## Overview

The section convers the configuration of JMAP including resource limits as well as
E-mail and Mailbox settings.


## Request Limits

The following parameters control different limits to be enforced on JMAP requests:

- ``max-size-upload``: Maximum file size in bytes that can be uploaded. Defaults to 50000000 (~50 MB).
- ``max-size-request``: Maximum JMAP request size in bytes. Defaults to 10000000 (~10 MB).
- ``max-calls-in-request``: Maximum number of method calls in a JMAP request. Defaults to 16.
- ``max-objects-in-get``: Maximum number of objects that can be fetched in a single JMAP Get call. Defaults to 500.
- ``max-objects-in-set``: Maximum number of objects that can be created, updated or destroyed in a single JMAP Set call. Defaults to 500.
- ``query-max-results``: Maximum number of results to return in a JMAP Query response. Defaults to 5000.
- ``changes-max-results``: Maximum number of results to return in a JMAP Changes response. Defaults to 5000.

Example:

```
max-size-upload: 50000000 # bytes
max-size-request: 10000000 # bytes
max-calls-in-request: 16
max-objects-in-get: 500
max-objects-in-set: 500
query-max-results: 5000
changes-max-results: 5000
```

## E-mail Settings

Stalwart JMAP includes a full-text search engine that automatically detects the language
of each email when the ``Content-Language`` MIME header is absent. In cases where automatically detecting
the language of a message is not possible (for example, en email containing just an ambiguous very short sentence),
you can set a default language with the ``default-language`` setting. This parameter accepts a valid 
[ISO 639-1 language code](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) and its default value is ``en`` (English). 
Example:

```
default-language: en
```

Additionally, the following limits can be enforced on JMAP Email objects:

- ``mail-max-size``: Maximum allowed size in bytes of an e-mail message. Default to 104857600 bytes (100 MB).
- ``mail-attachments-max-size``: Maximum allowed size in bytes of an e-mail attachment. Default to 50000000 bytes (~50 MB).
- ``mail-import-max-items``: Maximum number of messages that can be imported on a single Email/import JMAP request. Default to 5 items.
- ``mail-parse-max-items``: Maximum numbers of messages that can be parsed by a single Email/parse JMAP request. Default to items.

Example:

```
mail-max-size: 104857600 # bytes
mail-attachments-max-size: 50000000 # bytes
mail-import-max-items: 5
mail-parse-max-items: 5
```

## Mailbox Settings

The following limits can be set for JMAP mailboxes:

- ``mailbox-name-max-len``: Maximum length of a mailbox name. Defaults to 255 characters.
- ``mailbox-max-total``: Maximum number of mailboxes that a user can create. Defaults to 1000.
- ``mailbox-max-depth``: Maximum nesting levels for mailboxes. Defaults to 10 levels.

Example:

```
mailbox-name-max-len: 255
mailbox-max-total: 1000
mailbox-max-depth: 10
```
