---
title: "Access Control Lists"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "manage"
    identifier: "acl"
weight: 306
toc: true
---

## Overview

Stalwart JMAP supports Access Control Lists (ACLs) which allow users to share e-mails and
mailboxes with other accounts or groups. However, at the moment the JMAP Sharing specification
is still a [draft](https://datatracker.ietf.org/doc/draft-ietf-jmap-sharing/) and, for that reason,
it is not yet possible to share information or modify ACLs over JMAP.

Until JMAP Sharing becomes a draft, it is recommended to use [Stalwart IMAP](/imap/) to manage
ACLs and share information between accounts. Stalwart IMAP includes full support for the [IMAP4 Access Control List (ACL) Extension](https://www.rfc-editor.org/rfc/rfc4314)
which allows IMAP4 clients to fully manage the access control lists of an account.

