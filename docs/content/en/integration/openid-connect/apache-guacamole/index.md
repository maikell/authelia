---
title: "Apache Guacamole"
description: "Integrating Apache Guacamole with the Authelia OpenID Connect Provider."
lead: ""
date: 2022-07-31T13:09:05+10:00
draft: false
images: []
menu:
integration:
parent: "openid-connect"
weight: 620
toc: true
community: true
---

## Tested Versions

* [Authelia]
  * [v4.36.3](https://github.com/authelia/authelia/releases/tag/v4.36.2)
* [Apache Guacamole]
  * __UNKNOWN__

## Before You Begin

### Common Notes

1. You are *__required__* to utilize a unique client id for every client.
2. The client id on this page is merely an example and you can theoretically use any alphanumeric string.
3. You *__should not__* use the client secret in this example, We *__strongly recommend__* reading the
   [Generating Client Secrets] guide instead.

[Generating Client Secrets]: ../specific-information.md#generating-client-secrets

### Assumptions

This example makes the following assumptions:

* __Application Root URL:__ `https://guacamole.example.com`
* __Authelia Root URL:__ `https://auth.example.com`
* __Client ID:__ `guacamole`
* __Client Secret:__ `guacamole_client_secret`

## Configuration

### Application

To configure [Apache Guacamole] to utilize Authelia as an [OpenID Connect 1.0] Provider use the following configuration:

```yaml
openid-client-id: guacamole
openid-scope: openid profile groups email
openid-issuer: https://auth.example.com
openid-jwks-endpoint: https://auth.example.com/jwks.json
openid-authorization-endpoint: https://auth.example.com/api/oidc/authorization?state=1234abcedfdhf
openid-redirect-uri: https://guacamole.example.com
openid-username-claim-type: preferred_username
openid-groups-claim-type: groups
```

### Authelia

The following YAML configuration is an example __Authelia__
[client configuration](../../../configuration/identity-providers/open-id-connect.md#clients) for use with
[Apache Guacamole] which will operate with the above example:

```yaml
- id: guacamole
  description: Apache Guacamole
  secret: '$plaintext$guacamole_client_secret'
  public: false
  authorization_policy: two_factor
  redirect_uris:
    - https://guacamole.example.com
  scopes:
    - openid
    - profile
    - groups
    - email
  response_types:
    - id_token
  grant_types:
    - implicit
  userinfo_signing_algorithm: none
```

## See Also

* [Apache Guacamole OpenID Connect Authentication Documentation](https://guacamole.apache.org/doc/gug/openid-auth.html)

[Authelia]: https://www.authelia.com
[Apache Guacamole]: https://guacamole.apache.org/
[OpenID Connect 1.0]: ../../openid-connect/introduction.md




