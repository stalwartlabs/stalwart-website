---
title: "Import Accounts"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "migrate"
    identifier: "migrate-accounts"
weight: 402
toc: true
---

## Overview

Accounts can be quickly imported in bulk from a CSV file or standard input. The CLI command
to import user accounts into Stalwart JMAP is ``import accounts`` and accepts the following
options:

```bash
Bulk import user accounts

USAGE:
    stalwart-cli import accounts [OPTIONS] <PATH>

ARGS:
    <PATH>    Path to the CSV file, or '-' for stdin

OPTIONS:
    -c, --column-layout <COLUMN_LAYOUT>
            CSV column layout, default is 'email,secret,name,description,quota,timezone'

    -d, --delimiter <DELIMITER>
            CSV file delimiter, defaults to ','

    -h, --help
            Print help information

    -n, --no-domains
            Do not create domain names

    -w, --with-headers
            CSV has headers
```

## Fields

The following user account fields can be imported from a CSV file:

- ``email``: Account e-mail address, which is also used to log in.
- ``secret``: Account password in clear text.
- ``name``: Account full name.
- ``description``: Account description.
- ``quota``: Account quota in bytes.
- ``timezone``: User timezone.

## Layout

The column layout may be specified with the ``-c`` option and, if no layout is provided, the default 
``email,secret,name,description,quota,timezone`` is used. Alternatively, the ``-w`` option can be used
to obtain the column layout from the CSV file headers.

Columns in the CSV file are expected to be delimited by commas (``,``), to use a different delimiter, 
specify it with the ``-d`` parameter.

## Domains

By default the CLI tool will create the domain names for each e-mail address in the CSV file. To disable
this functionality, use the ``-n`` option. Please note that the account creation will fail if the domain
name for a new account does not exist already in the system.

## Usage

Create a CSV file with the accounts to be imported, for example:

```csv
email,secret,name,description
jane@example.org,123456,Jane Doe,Sales Manager
john@example.org,secretpass,John Doe,Sales Representative
bill@example.org,abcdefg,Bill Foobar,SysAdmin
```

Run the CLI tool providing the path to the CSV file:

```bash
$ stalwart-cli -u https://jmap.example.org import accounts -w new_accounts.csv
```

Or, use ``-`` as the filename to read the CSV file from standard input:

```bash
$ cat new_accounts.csv | stalwart-cli -u https://jmap.example.org import accounts -w
```

Before creating the accounts, the CLI tool will ask for confirmation:

```
Will import 3 accounts and 1 domain names. Press Enter to go ahead or CTRL+C to cancel.
```

If everything looks fine, press *ENTER* to proceed with the import:

```
Successfully imported 3 accounts.
```
