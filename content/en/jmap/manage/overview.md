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
    parent: "manage"
    identifier: "manage-overview"
weight: 301
toc: true
---

## CLI

The Stalwart Command Line Interface (CLI) allows system administrators to perform tasks
such as creating user accounts and migrating information. Future versions of Stalwart JMAP
will include a web-based management interface, but for the time being the CLI is the main
tool to manage a Stalwart JMAP server.

## Installation

The CLI tool should be aleady installed on the server where Stalwart JMAP server is running at 
``/usr/local/stalwart-jmap/bin/stalwart-cli``. 
If you would like to install the CLI on a different computer, follow the instructions for 
your platform below:

- **Linux / MacOS**: 

    ```bash
    curl --proto '=https' --tlsv1.2 -sSf https://jmap-cli.stalw.art/install.sh | sh
    ```
    Once the installation is completed, the CLI tool will be available in your home directory at ``$HOME/.stalwart/stalwart-cli``. You may add the
    ``$HOME/.stalwart`` directory to your ``PATH`` environment variable or move the ``stalwart-cli`` binary to a location that is already
    on your ``PATH`` variable.

- **Windows**: 
  
    Download the CLI tool directly from [here](https://github.com/stalwartlabs/jmap-server-cli/releases/latest/download/stalwart-cli-x86_64-pc-windows-msvc.zip).

## Usage

The default location of the Stalwart CLI is ``/usr/local/stalwart-jmap/bin/stalwart-cli`` (or ``$HOME/.stalwart/stalwart-cli``
when installed manually). When executed without any parameters, the CLI tool prints a brief help page such as this one:

```bash
$ stalwart-cli

Stalwart JMAP Server CLI

USAGE:
    stalwart-cli [OPTIONS] --url <URL> <SUBCOMMAND>

OPTIONS:
    -c, --credentials <CREDENTIALS>    Authentication credentials
    -h, --help                         Print help information
    -u, --url <URL>                    JMAP server base URL
    -V, --version                      Print version information

SUBCOMMANDS:
    account    Manage user accounts
    domain     Manage domains
    group      Manage groups
    help       Print this message or the help of the given subcommand(s)
    import     Import accounts and domains
    list       Manage mailing lists
```

The CLI tool expects two required arguments: the base URL of your Stalwart JMAP server (which is 
specified with the ``-u`` option) server and the system administrator credentials (which 
may be specified with the ``-c`` option or at the prompt).

For example, to list all existing accounts:

```bash
$ stalwart-cli -u https://jmap.example.org -c PASSWORD account list
```

## Authentication

In order to be able to perform management tasks, the CLI tool requires you to be authenticated using
the system administrator credentials. Authentication with your JMAP server can be done either
using the **Basic** or **OAuth** mechanisms. However, for security reasons, it is always
preferable to authenticate using OAuth. 

### OAuth

To use OAuth authentication, run the ``stalwart-cli`` command ommitting the ``-c`` option. The CLI tool
will then ask at the prompt if you would like to authenticate using OAuth:

```bash
$ stalwart-cli -u https://jmap.example.org account list

Enter admin credentials or press [ENTER] to use OAuth: 
```

Press __ENTER__ to start the OAuth authentication flow and obtain the authorization code:

```bash
Authenticate this request using code HY5E-UUG2 at https://jmap.example.org/auth. Please ENTER when done.
```

On your browser, go to ``https://jmap.example.org/auth`` and enter the provided code (in this example,
``HY5E-UUG2``) as well as the system administrator username and password:

![OAuth Login](images/oauth_login.png)

If the login is successful, the following message will be displayed:

![OAuth Login](images/oauth_success.png)

Go back to the terminal where ``stalwart-cli`` is being executed and press ``ENTER`` to execute
the command:

```bash
+-----------------+------------------+-------------+-------+
| E-mail          | Name             | Description | Quota |
+-----------------+------------------+-------------+-------+
| admin           | Administrator    |             |       |
+-----------------+------------------+-------------+-------+


1 record found.
```

### Basic

Authenticating using the Basic mechanism is done directly from the command line with the ``-c`` argument.
The credentials have to be specified as ``account_name:password`` and, if the account name is omitted, the 
default system administrator account ``admin`` is used.
For example, to authenticate with ``postmaster@example.org`` and password ``secret_pass``:

```bash
$ stalwart-cli -u https://jmap.example.org -c postmaster@example.org:secret_pass account list
```

**Note**: Avoid using the ``-c`` argument to provide the administrator credentials as these
will be recorded in the terminal's history. Instead, type the password at the prompt:

```bash
$ stalwart-cli -u https://jmap.example.org account list

Enter admin credentials or press [ENTER] to use OAuth: ******
```
