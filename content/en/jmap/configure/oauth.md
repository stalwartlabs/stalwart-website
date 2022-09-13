---
title: "OAuth Settings"
description: ""
date: 2022-08-28T12:49:20Z
lastmod: 2022-08-28T12:49:20Z
draft: false
images: []
type: docs
menu:
  jmap:
    parent: "configure"
    identifier: "oauth"
weight: 206
toc: true
---

## Overview

The OAuth authorization framework enables a third-party application such as an e-mail client to obtain limited 
access to a Stalwart JMAP account. For security reasons, it is strongly recommended to always use OAuth to
authenticate your users and, if possible, disable the ``Basic`` authentication scheme altogether.

OAuth 2.0 is automatically enabled in Stalwart JMAP and does not require any special configurations to function.
There are however some settings that can be tweaked to control when tokens expire, how often they need to be renewed,
as well as other security settings.

## Authorization Flows

In order to facilitate and encourage OAuth adoption by system administrators, Stalwart JMAP includes support for both 
the __OAuth 2.0 Authorization Code Flow__ ([RFC6749](https://www.rfc-editor.org/rfc/rfc6749.html)) and 
__OAuth 2.0 Device Authorization Flow__ ([RFC8628](https://www.rfc-editor.org/rfc/rfc8628)).
Deciding which authorization flow to use is up to the client accessing the JMAP account.

### Authorization Code

The __authorization code flow__ is the most commonly used OAuth flow in which a code is
obtained by using an authorization server (Stalwart JMAP in this case) as an intermediary between the 
client and JMAP account owner.
Instead of requesting authorization directly from the account owner, the client directs the account
owner to Stalwart JMAP where they are authenticated and then redirected back to the client with 
the authorization code.

### Device Authorization

The __device authorization flow__ is designed for Internet-connected devices that either lack a browser 
to perform a user-agent-based authorization or are input constrained to the extent that requiring the 
user to input text in order to authenticate during the authorization flow is impractical.  It enables 
OAuth clients on such devices (like smart TVs, media consoles, digital picture frames, and printers) to 
obtain user authorization to access protected resources by using a user agent on a separate device.

## OAuth Tokens

Once a user is successfully authenticated using OAuth, Stalwart JMAP will issue the client two unique codes known 
as the *access* and *refresh* tokens. An *access token* grants the client access to a JMAP account for a relatively short 
period of time (usually one hour) and then expires. Once it expires, the *refresh token* has to be used to obtain
a new *access token* in order to regain access to the account. This process is repeated until either the 
refresh token expires or it is revoked by the JMAP account owner or system administrator.

### Encryption

All tokens issued by Stalwart JMAP are cryptographically signed using a mechanism inspired by 
[PASETO security tokens](https://paseto.io/):

- An **encryption key** is derived using the [BLAKE3](https://en.wikipedia.org/wiki/BLAKE_(hash_function)#BLAKE3)
  cryptographic hash function combining the principal encription key (configurable with the `encryption-key`
  parameter) with the device id, the expiration time, account details and an [Argon2](https://en.wikipedia.org/wiki/Argon2)
  hash of the JMAP account's password.
- A **nonce** is generated using a BLAKE3 hash of the user details and the expiration time of the token.
- Finally, the derived **encryption key** and **nonce** are employed to digitally sign the token using 
  [AES-256-GCM-SIV](https://www.rfc-editor.org/rfc/rfc8452).

During installation, a 512 bits long encryption key is automatically generated for you. If you wish
to replace it, you can generate a new one using OpenSSL or alternatively in Linux systems using the
command ``LC_ALL=C tr -dc '[:alpha:]' < /dev/urandom | head -c 64``. Once you have your new key, update the ``encryption-key``
parameter, for example:

```
encryption-key: sDRvvROsJmRUnjIfcUiDUSaAxdQpfixuLvdwlSffptaxUnSQZALenZSYUPQQByUI
```

It is imporant to be aware that:

- If the encription key is changed, all existing OAuth tokens will be **immediately revoked**.
- On distributed systems, all nodes have to use the **exact same encription key**. Otherwise,
  tokens issued in one node will not be valid in other nodes.
- The encryption key **must be kept private**, make sure that only the Stalwart JMAP process
  has access to the configuration file where it is stored. In Unix systems this can be done
  with the commands:
  
   ```
   sudo chown stalwart-jmap /usr/local/stalwart-jmap/etc/config.yml
   sudo chmod 600 /usr/local/stalwart-jmap/etc/config.yml
   ``` 

### Expiration

System administrators can configure the expiration time of both access and
refresh tokens as well as the authorization codes issued to users and devices:

- ``oauth-token-expiry``: Expiration time of an OAuth access token in seconds, defaults to 3600 seconds (1 hour).
- ``oauth-refresh-token-expiry``: Expiration time of an OAuth refresh token in seconds. Defaults to 2592000 seconds (1 month).
- ``oauth-refresh-token-renew``:  Number of remaining seconds in a refresh token before a new one is issued to the client. Defaults to 345600 seconds (4 days).
- ``oauth-user-code-expiry``: Expiration time in seconds of a user code issued by the device authentication flow. Defaults to 1800 seconds (30 minutes).
- ``oauth-auth-code-expiry``: Expiration time in seconds of an authorization code issued by the authorization code flow. Defaults to 600 seconds (10 minutes).
- ``oauth-max-attempts``: Number of failed login attempts before an authorization code is invalidated. Defaults to 3 failed attempts.

Example:

```
oauth-user-code-expiry: 1800 # secs
oauth-auth-code-expiry: 600 # secs
oauth-token-expiry: 3600 # secs
oauth-refresh-token-expiry: 2592000  # secs
oauth-refresh-token-renew: 345600  # secs
oauth-max-attempts: 3
```

### Revocation

Access and refresh tokens issued by Stalwart JMAP can be revoked by both account owners
and system administrators simply by changing the account's password.
In the near future it will also be possible to revoke individual tokens issued to a
specific OAuth client id.

## OAuth Endpoints

OAuth clients conforming to [RFC8414](https://www.rfc-editor.org/rfc/rfc8414.html) can obtain
all available OAuth endpoints by requesting the authorization server metadata which is published
at ``/.well-known/oauth-authorization-server``. Clients that do not support RFC8414 need to be manually
configured to use the appropriate endpoint:

- ``/auth/token``: Token endpoint, used by all flows to obtain and refresh access tokens.
- ``/auth/device``: Device authorization endpoint, used by the device flow.

Once the OAuth client obtains the authorization code, users are then authenticated on any
of these two endpoints:

- ``/auth/code``: Used by the authorization code flow to authenticate accounts.
- ``/auth``: Used by the device authorization flow to authenticate accounts.

## Conformed RFCs

- [RFC 6749 - The OAuth 2.0 Authorization Framework](https://www.rfc-editor.org/rfc/rfc6749.html)
- [RFC 6750 - The OAuth 2.0 Authorization Framework: Bearer Token Usage](https://datatracker.ietf.org/doc/html/rfc6750)
- [RFC 8414 - OAuth 2.0 Authorization Server Metadata](https://www.rfc-editor.org/rfc/rfc8414.html)
- [RFC 8628 - OAuth 2.0 Device Authorization Grant](https://www.rfc-editor.org/rfc/rfc8628)
