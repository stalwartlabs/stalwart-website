---
title: "Groups"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "manage"
    identifier: "groups"
weight: 304
toc: true
---

## Overview

Groups represent units within an organization and can have as members user accounts or other
groups. Each group is identified with an e-mail address which is able to receive messages from the outside.
E-mail folders and messages owned by a group are accessible by all members of the group.

Group management is done using the CLI command ``group`` which, if executed without parameters,
returns the available subcommands:

```bash
$ stalwart-cli -u https://jmap.example.org group

Manage groups

USAGE:
    stalwart-cli --url <URL> group <SUBCOMMAND>

OPTIONS:
    -h, --help    Print help information

SUBCOMMANDS:
    add-members       Add members to a group
    create            Create a group
    display           Display an existing group
    help              Print this message or the help of the given subcommand(s)
    list              List all groups
    remove-members    Remove members from a group
    update            Update an existing group
```

## Create

Group creation is done using the ``group create`` subcommand. It accepts the following arguments:

```bash
Create a group

USAGE:
    stalwart-cli group create [OPTIONS] <EMAIL> <NAME>

ARGS:
    <EMAIL>    Group email address
    <NAME>     Name

OPTIONS:
    -d, --description <DESCRIPTION>    Description
    -h, --help                         Print help information
```

For example, to create a Sales Team with e-mail address ``sales@example.org``:

```bash
$ stalwart-cli -u https://jmap.example.org group create sales@example.org "Sales Team"

Group 'sales@example.org' successfully created.
```

## Display

Group details are obtained using the ``group display`` subcommand. For example:

```bash
$ stalwart-cli -u https://jmap.example.org group display sales@example.org

+-------------+-------------------+
| E-mail      | sales@example.org |
+-------------+-------------------+
| Name        | Sales Team        |
+-------------+-------------------+
| Description |                   |
+-------------+-------------------+
| Members     |                   |
+-------------+-------------------+
```

## List

Groups are listed using the ``group list`` subcommand. It accepts an optional parameter
to filter the group list by a keyword. For example:

```bash
$ stalwart-cli -u https://jmap.example.org group list

+---------------------+--------------+-------------+
| E-mail              | Name         | Description |
+---------------------+--------------+-------------+
| sales@example.org   | Sales Team   |             |
+---------------------+--------------+-------------+
| support@example.org | Support Team |             |
+---------------------+--------------+-------------+


2 records found.
```

Or using the filter:

```bash
$ stalwart-cli -u https://jmap.example.org group list sales

+---------------------+--------------+-------------+
| E-mail              | Name         | Description |
+---------------------+--------------+-------------+
| sales@example.org   | Sales Team   |             |
+---------------------+--------------+-------------+


1 record found.
```

## Update

Group updates are done using the ``group update`` subcommand. It accepts the following arguments:

```bash
Update an existing group

USAGE:
    stalwart-cli group update [OPTIONS] <EMAIL>

ARGS:
    <EMAIL>    Group email address

OPTIONS:
    -d, --description <DESCRIPTION>    Description
    -h, --help                         Print help information
    -n, --name <NAME>                  Name
```

For example, to change a group's name:

```bash
$ stalwart-cli -u https://jmap.example.org group update sales@example.org -n "Global Sales Team"

Group 'sales@example.org' successfully updated.
```

### Members

Group members are managed using the subcommands ``group add-members`` and
``group remove-members``. Both subcommands expect a list of user or group e-mail addresses to add or remove.

For example, to add multiple members to a group:

```bash
$ stalwart-cli -u https://jmap.example.org group add-members sales@example.org \
    john@example.org jane@example.org

Group 'sales@example.org' successfully updated.
```

And, to remove a member from a group:

```bash
$ stalwart-cli -u https://jmap.example.org group remove-members sales@example.org \
    john@example.org

Group 'sales@example.org' successfully updated.
```

## Delete

Groups are removed using the ``group delete`` subcommand. For example:

```bash
$ stalwart-cli -u https://jmap.example.org group delete jdoe@example.org

Group 'jdoe@example.org' successfully deleted.
```
