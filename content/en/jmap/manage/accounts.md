---
title: "Accounts"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "manage"
    identifier: "accounts"
weight: 302
toc: true
---

## Overview

Account management is done using the CLI command ``account`` which, if executed without parameters,
returns the available subcommands:

```bash
$ stalwart-cli -u https://jmap.example.org account

Manage user accounts

USAGE:
    stalwart-cli --url <URL> account <SUBCOMMAND>

OPTIONS:
    -h, --help    Print help information

SUBCOMMANDS:
    add-alias       Add e-mail aliases to a user account
    create          Create a new user account
    delete          Delete an existing user account
    display         Display an existing user account
    help            Print this message or the help of the given subcommand(s)
    list            List all user accounts
    remove-alias    Add e-mail aliases to a user account
    update          Update an existing user account
```

## Create

User account creation is done using the ``account create`` subcommand. It accepts the following arguments:

```bash
Create a new user account

USAGE:
    stalwart-cli account create [OPTIONS] <EMAIL> <PASSWORD> <NAME>

ARGS:
    <EMAIL>       Login email address
    <PASSWORD>    Password
    <NAME>        Account Name

OPTIONS:
    -d, --description <DESCRIPTION>        Account description
    -e, --email-aliases <EMAIL_ALIASES>    E-mail address aliases
    -h, --help                             Print help information
    -q, --quota <QUOTA>                    Quota in bytes
    -t, --timezone <TIMEZONE>              Timezone
```

For example, to create the user John Doe with e-mail address ``john@example.org`` (which is also the login name) and password ``secret_pass``:

```bash
$ stalwart-cli -u https://jmap.example.org account create jdoe@example.org secret_pass "John Doe"

Account 'jdoe@example.org' successfully created.
```

## Display

User account details are obtained using the ``account display`` subcommand. For example:

```bash
$ stalwart-cli -u https://jmap.example.org account display jdoe@example.org

+-------------+------------------+
| E-mail      | jdoe@example.org |
+-------------+------------------+
| Name        | John Doe         |
+-------------+------------------+
| Description |                  |
+-------------+------------------+
| Quota       |                  |
+-------------+------------------+
| Timezone    |                  |
+-------------+------------------+
| Aliases     |                  |
+-------------+------------------+
```

## List

User accounts are listed using the ``account list`` subcommand. It accepts an optional parameter
to filter the accounts list by a keyword. For example:

```bash
$ stalwart-cli -u https://jmap.example.org account list

+------------------+--------------------+-------------+-------+
| E-mail           | Name               | Description | Quota |
+------------------+--------------------+-------------+-------+
| admin            | Administrator      |             |       |
+------------------+--------------------+-------------+-------+
| jane@example.org | Jane doe           |             |       |
+------------------+--------------------+-------------+-------+
| jdoe@example.org | John Doe           |             |       |
+------------------+--------------------+-------------+-------+


3 records found.
```

Or using the filter:

```bash
$ stalwart-cli -u https://jmap.example.org account list john

+------------------+--------------------+-------------+-------+
| E-mail           | Name               | Description | Quota |
+------------------+--------------------+-------------+-------+
| jdoe@example.org | John Doe           |             |       |
+------------------+--------------------+-------------+-------+


1 record found.
```

## Update

User account updates are done using the ``account update`` subcommand. It accepts the following arguments:

```bash
Update an existing user account

USAGE:
    stalwart-cli account update [OPTIONS] <EMAIL>

ARGS:
    <EMAIL>    Account email address

OPTIONS:
    -d, --description <DESCRIPTION>    Update account description
    -h, --help                         Print help information
    -n, --name <NAME>                  Update account name
    -p, --password <PASSWORD>          Update password
    -q, --quota <QUOTA>                Update quota in bytes
    -t, --timezone <TIMEZONE>          Update timezone
```

For example, to change an account password:

```bash
$ stalwart-cli -u https://jmap.example.org account update jdoe@example.org -p new_pass

Account 'jdoe@example.org' successfully updated.
```

### Aliases

E-mail aliases for an account are managed using the subcommands ``account add-alias`` and
``account remove-alias``. Both subcommands expect a list of e-mail addresses aliases to add or remove.

For example, to add multiple e-mail aliases to an account:

```bash
$ stalwart-cli -u https://jmap.example.org account add-alias jdoe@example.org \
    john@example.org john.doe@example.org

Account 'jdoe@example.org' successfully updated.
```

And, to remove an e-mail alias:

```bash
$ stalwart-cli -u https://jmap.example.org account remove-alias jdoe@example.org \
    john@example.org john.doe@example.org

Account 'jdoe@example.org' successfully updated.
```

## Delete

User accounts are removed using the ``account delete`` subcommand. For example:

```bash
$ stalwart-cli -u https://jmap.example.org account delete jdoe@example.org

Account 'jdoe@example.org' successfully deleted.
```
