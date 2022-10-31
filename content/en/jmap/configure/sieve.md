---
title: "Sieve Filters"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "rate-limit"
weight: 210
toc: true
---

## Overview

Sieve ([RFC5228](https://www.rfc-editor.org/rfc/rfc5228.html)) is a scripting language for filtering email messages at or around the time of final delivery.
It is suitable for running on a mail server where users may not be allowed to execute arbitrary programs 
as it has no user-controlled loops or the ability to run external programs.
Sieve is a data-driven programming language, similar to earlier email filtering languages such as procmail and 
maildrop, and earlier line-oriented languages such as sed and AWK: it specifies conditions to match and actions 
to take on matching.

Stalwart JMAP includes support for [JMAP for Sieve Scripts](https://www.ietf.org/archive/id/draft-ietf-jmap-sieve-12.html) as well as [ManageSieve](https://datatracker.ietf.org/doc/html/rfc5804) (via [Stalwart IMAP](/imap/configure/sieve/)), which allows
users to upload and manage their Sieve scripts.

### Extensions

Stalwart JMAP includes support for [all existing Sieve extensions](https://www.iana.org/assignments/sieve-extensions/sieve-extensions.xhtml) which are enabled
by default. However, system administrators might want to disable certain extensions such as *enotify* which allow users to send outgoing emails from
a script, or extensions that allow modifying the contents of a message such as *editheader*, *replace* or *enclose*.

Disabling Sieve extensions can be done by setting the ``sieve-disable-capabilities`` with the capabilities to disable separated by a space, for example:

```
sieve-disable-capabilities: editheader replace enclose enotify
```

### Compiler Parameters

Sieve scripts are compiled immediately after they are uploaded in order to speed up their execution during 
the message delivery stage. The following parameters enforce different limits on Sieve scripts at compile time:

- ``sieve-max-script-size``: Maximum size of a Sieve script in bytes. Defaults to ``1048576`` (1MB).
- ``sieve-max-string-size``: Maximum size of a constant string. Defaults to ``4096``.
- ``sieve-max-variable-name-size``: Maximum length of a variable name. Defaults to ``32``.
- ``sieve-max-nested-blocks``: Maximum number of nested ``if`` / ``elsif`` / ``else`` blocks. Defaults to ``15``.
- ``sieve-max-nested-tests``: Maximum number of nested tests. Defaults to ``15``.
- ``sieve-max-nested-foreverypart``: Maximum number of nested ``foreverypart`` loops. Defaults to ``3``.
- ``sieve-max-match-variables``: Maximum number of ``matches`` and ``regex`` variables that can be captured. Defaults to ``30``.
- ``sieve-max-local-variables``: Maximum number of local variables that can be in scope at any given time. Defaults to ``128``.
- ``sieve-max-header-size``: Maximum length of an RFC822 header value. Defaults to ``1024``.
- ``sieve-max-includes``: Maximum number of ``include`` instructions per script. Defaults to ``3``.
 

### Runtime Parameters

Sieve scripts are safely executed in a controlled sandbox that ensures that scripts do not exceed or abuse their
allocated system resources. The following parameters enforce different limits on Sieve scripts at execution time:

- ``sieve-cpu-limit``: Maximum number of instructions that a script can execute (including instructions from ``include`` scripts). Defaults to ``5000``.
- ``sieve-max-nested-includes``: Maximum number of nested ``include``. Defaults to ``3``.
- ``sieve-max-variable-size``: Maximum length of a variable, after this limit variable contents are truncated. Defaults to ``4096``.
- ``sieve-max-redirects``: Maximum number of message redirections per execution. Defaults to ``1``.
- ``sieve-max-received-headers``: Maximum number of ``Received`` headers before a message is considered to be in a loop. Defaults to ``10``.
- ``sieve-max-header-size``: Maximum length of a message header. Defaults to ``1024``.
- ``sieve-max-outgoing-messages``: Maximum number of outgoing e-mail messages that can be sent from a script including vacation responses, notifications and redirects. Defaults to ``3``.
- ``sieve-protected-headers``: List of protected headers that cannot be deleted or added using the ``editheader`` extension. Defaults to ``Original-Subject Original-From Received Auto-Submitted``.
- ``sieve-default-vacation-expiry``: Default expire time in seconds of a ``vacation`` entry. Defaults to ``2592000`` (30 days).
- ``sieve-default-duplicate-expiry``: Default expire time in seconds of a ``duplicate`` entry. Defaults to ``604800`` (7 days).
- ``sieve-notification-uris``: List of supported URIs (separated by spaces) by the ``enotify`` extension. Defaults to ``mailto``.
- ``sieve-vacation-default-subject``: Default subject of vacation responses. Defaults to ``Automated reply``.
- ``sieve-vacation-subject-prefix``: Default subject prefix of vacation responses. Defaults to ``Auto: ``.

### Conformed RFCs

- [RFC 5228 - Sieve: An Email Filtering Language](https://datatracker.ietf.org/doc/html/rfc5228)
- [RFC 3894 - Copying Without Side Effects](https://datatracker.ietf.org/doc/html/rfc3894)
- [RFC 5173 - Body Extension](https://datatracker.ietf.org/doc/html/rfc5173)
- [RFC 5183 - Environment Extension](https://datatracker.ietf.org/doc/html/rfc5183)
- [RFC 5229 - Variables Extension](https://datatracker.ietf.org/doc/html/rfc5229)
- [RFC 5230 - Vacation Extension](https://datatracker.ietf.org/doc/html/rfc5230)
- [RFC 5231 - Relational Extension](https://datatracker.ietf.org/doc/html/rfc5231)
- [RFC 5232 - Imap4flags Extension](https://datatracker.ietf.org/doc/html/rfc5232)
- [RFC 5233 - Subaddress Extension](https://datatracker.ietf.org/doc/html/rfc5233)
- [RFC 5235 - Spamtest and Virustest Extensions](https://datatracker.ietf.org/doc/html/rfc5235)
- [RFC 5260 - Date and Index Extensions](https://datatracker.ietf.org/doc/html/rfc5260)
- [RFC 5293 - Editheader Extension](https://datatracker.ietf.org/doc/html/rfc5293)
- [RFC 5429 - Reject and Extended Reject Extensions](https://datatracker.ietf.org/doc/html/rfc5429)
- [RFC 5435 - Extension for Notifications](https://datatracker.ietf.org/doc/html/rfc5435)
- [RFC 5463 - Ihave Extension](https://datatracker.ietf.org/doc/html/rfc5463)
- [RFC 5490 - Extensions for Checking Mailbox Status and Accessing Mailbox Metadata](https://datatracker.ietf.org/doc/html/rfc5490)
- [RFC 5703 - MIME Part Tests, Iteration, Extraction, Replacement, and Enclosure](https://datatracker.ietf.org/doc/html/rfc5703)
- [RFC 6009 - Delivery Status Notifications and Deliver-By Extensions](https://datatracker.ietf.org/doc/html/rfc6009)
- [RFC 6131 - Sieve Vacation Extension: "Seconds" Parameter](https://datatracker.ietf.org/doc/html/rfc6131)
- [RFC 6134 - Externally Stored Lists](https://datatracker.ietf.org/doc/html/rfc6134)
- [RFC 6558 - Converting Messages before Delivery](https://datatracker.ietf.org/doc/html/rfc6558)
- [RFC 6609 - Include Extension](https://datatracker.ietf.org/doc/html/rfc6609)
- [RFC 7352 - Detecting Duplicate Deliveries](https://datatracker.ietf.org/doc/html/rfc7352)
- [RFC 8579 - Delivering to Special-Use Mailboxes](https://datatracker.ietf.org/doc/html/rfc8579)
- [RFC 8580 - File Carbon Copy (FCC)](https://datatracker.ietf.org/doc/html/rfc8580)
- [RFC 9042 - Delivery by MAILBOXID](https://datatracker.ietf.org/doc/html/rfc9042)
- [REGEX-01 - Regular Expression Extension (draft-ietf-sieve-regex-01)](https://www.ietf.org/archive/id/draft-ietf-sieve-regex-01.html)
