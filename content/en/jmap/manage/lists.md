---
title: "Mailing Lists"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "manage"
    identifier: "lists"
weight: 305
toc: true
---

## Overview

Mailing lists are collections of e-mail addresses that are used to send messages to multiple 
user accounts simultaneously.
Mailing list management is done using the CLI command ``list`` which, if executed without parameters,
returns the available subcommands:

```bash
$ stalwart-cli -u https://jmap.example.org list

Manage mailing lists

USAGE:
    stalwart-cli --url <URL> list <SUBCOMMAND>

OPTIONS:
    -h, --help    Print help information

SUBCOMMANDS:
    add-members       Add members to a mailing list
    create            Create a new mailing list
    display           Display an existing mailing list
    help              Print this message or the help of the given subcommand(s)
    list              List all mailing lists
    remove-members    Remove members from a mailing list
    update            Update an existing mailing list
```

## Create

Mailing list creation is done using the ``list create`` subcommand. It accepts the following arguments:

```bash
Create a new mailing list

USAGE:
    stalwart-cli list create [OPTIONS] <EMAIL> <NAME>

ARGS:
    <EMAIL>    List email address
    <NAME>     Name

OPTIONS:
    -d, --description <DESCRIPTION>    Description
    -h, --help                         Print help information
```

For example, to create a mailing list with e-mail address ``support@example.org``:

```bash
$ stalwart-cli -u https://jmap.example.org list create support@example.org "Support List"

List 'support@example.org' successfully created.
```

## Display

Mailing list details are obtained using the ``list display`` subcommand. For example:

```bash
$ stalwart-cli -u https://jmap.example.org list display support@example.org

+-------------+---------------------+
| E-mail      | support@example.org |
+-------------+---------------------+
| Name        | Support List        |
+-------------+---------------------+
| Description |                     |
+-------------+---------------------+
| Members     |                     |
+-------------+---------------------+
```

## List

Mailing lists are listed using the ``list list`` subcommand. It accepts an optional parameter
to filter the list list by a keyword. For example:

```bash
$ stalwart-cli -u https://jmap.example.org list list

+-----------------------+--------------+-------------+
| E-mail                | Name         | Description |
+-----------------------+--------------+-------------+
| corporate@example.org | Corporate    |             |
+-----------------------+--------------+-------------+
| support@example.org   | Support List |             |
+-----------------------+--------------+-------------+


2 records found.
```

Or using the filter:

```bash
$ stalwart-cli -u https://jmap.example.org list list support

+-----------------------+--------------+-------------+
| E-mail                | Name         | Description |
+-----------------------+--------------+-------------+
| supporo@example.org   | Support List |             |
+-----------------------+--------------+-------------+


1 record found.
```

## Update

Mailing list updates are done using the ``list update`` subcommand. It accepts the following arguments:

```bash
Update an existing mailing list

USAGE:
    stalwart-cli list update [OPTIONS] <EMAIL>

ARGS:
    <EMAIL>    List email address

OPTIONS:
    -d, --description <DESCRIPTION>    Description
    -h, --help                         Print help information
    -n, --name <NAME>                  Name
```

For example, to change a list's name:

```bash
$ stalwart-cli -u https://jmap.example.org list update support@example.org -n "Global Support Team"

List 'support@example.org' successfully updated.
```

### Members

Mailing list members are managed using the subcommands ``list add-members`` and
``list remove-members``. Both subcommands expect a list of e-mail addresses to add or remove.

For example, to add multiple members to a list:

```bash
$ stalwart-cli -u https://jmap.example.org list add-members support@example.org \
    john@example.org jane@example.org

List 'support@example.org' successfully updated.
```

And, to remove a member from a list:

```bash
$ stalwart-cli -u https://jmap.example.org list remove-members support@example.org \
    john@example.org

List 'support@example.org' successfully updated.
```

## Delete

Mailing lists are removed using the ``list delete`` subcommand. For example:

```bash
$ stalwart-cli -u https://jmap.example.org list delete jdoe@example.org

List 'jdoe@example.org' successfully deleted.
```
