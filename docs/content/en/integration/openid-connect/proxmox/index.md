---
title: "Proxmox"
description: "Integrating Proxmox with the Authelia OpenID Connect Provider."
lead: ""
date: 2022-06-15T17:51:47+10:00
draft: false
images: []
menu:
  integration:
    parent: "openid-connect"
weight: 620
toc: true
community: true
aliases:
  - /docs/community/oidc-integrations/proxmox.html
---

## Tested Versions

* [Authelia]
  * [v4.35.6](https://github.com/authelia/authelia/releases/tag/v4.35.6)
* [Proxmox]
  * 7.1-10

### Common Notes

1. You are *__required__* to utilize a unique client id for every client.
2. The client id on this page is merely an example and you can theoretically use any alphanumeric string.
3. You *__should not__* use the client secret in this example, We *__strongly recommend__* reading the
   [Generating Client Secrets] guide instead.

[Generating Client Secrets]: ../specific-information.md#generating-client-secrets

### Specific Notes

*__Important Note:__ [Proxmox] requires you create the Realm prior to adding the provider. This is not covered in this
guide.*

### Assumptions

This example makes the following assumptions:

* __Application Root URL:__ `https://proxmox.example.com`
* __Authelia Root URL:__ `https://auth.example.com`
* __Client ID:__ `proxmox`
* __Client Secret:__ `proxmox_client_secret`
* __Realm__ `authelia`

## Configuration

### Application

To configure [Proxmox] to utilize Authelia as an [OpenID Connect 1.0] Provider:

1. Visit Datacenter
2. Visit Permission
3. Visit Realms
4. Add an OpenID Connect Server
5. Set the following values:
   1. Issuer URL: `https://auth.example.com`
   2. Realm: `authelia`
   3. Client ID: `proxmox`
   4. Client Key: `proxmox_client_secret`
   5. Username Claim `preferred_username`
   6. Scopes: `openid profile email`
   7. Enable *Autocreate Users* if you want users to automatically be created in [Proxmox].

{{< figure src="proxmox.png" alt="Proxmox" width="736" style="padding-right: 10px" >}}

### Authelia

The following YAML configuration is an example __Authelia__
[client configuration](../../../configuration/identity-providers/open-id-connect.md#clients) for use with [Proxmox]
which will operate with the above example:

```yaml
- id: proxmox
  description: Proxmox
  secret: '$plaintext$proxmox_client_secret'
  public: false
  authorization_policy: two_factor
  redirect_uris:
    - https://proxmox.example.com
  scopes:
    - openid
    - profile
    - email
  userinfo_signing_algorithm: none
```

## See Also

* [Proxmox User Management Documentation](https://pve.proxmox.com/wiki/User_Management)

[Authelia]: https://www.authelia.com
[Proxmox]: https://www.proxmox.com/
[OpenID Connect 1.0]: ../../openid-connect/introduction.md
