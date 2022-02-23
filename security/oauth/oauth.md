# OAuth

**OAuth** (**O**pen **A**uthorization) 2.0 is the industry-standard protocol for authorization.

OAuth is used for granting websites or applications access to subset of user's information stored on another website.

OAuth is directly related to OpenID Connect (OIDC), since OIDC is an authentication layer built on top of OAuth 2.0.

## Table of contents

- [OAuth](#oauth)
  - [Table of contents](#table-of-contents)
  - [Terminology](#terminology)
    - [Client](#client)
    - [User](#user)
    - [Authorization server](#authorization-server)
    - [Resource server](#resource-server)
    - [Scope](#scope)
    - [Consent](#consent)
    - [Client ID](#client-id)
    - [Client secret](#client-secret)
    - [Redirect URI](#redirect-uri)
    - [Response type](#response-type)
    - [Authorization code](#authorization-code)

## Terminology

### Client

A **client** or an **app** or a **third-party app** is an application that uses OAuth to access some data or perform some actions on the *resource server* on behalf of the *user*.

### User

A **user** or a **resource owner** is an identity on behalf of which client wants to access some data or perform some actions on the *resource server*.

### Authorization server

An **authorization server** is a service that knows the user, where the user already has an account.

### Resource server

A **resource server** is an API or service on which client wants to access some data or perform some actions on behalf of the user.

> Sometimes the authorization server and the resource server are the same server. However, there are cases where they will *not* be the same server or even part of the same organization. For example, the authorization server might be a third-party service the resource server trusts.

### Scope

A **scope** is a set of granular permissions that client wants to obtain from authorization server to be able to access some data or perform some actions on the resource server on behalf of the user. Permissions are granted to client via user's *consent*.

### Consent

A **consent** is user's agreement to grant client permissions specified in the scope.

The authorization server takes the scopes the client is requesting, and verifies with the user whether or not he wants to give the client permission.

### Client ID

A **client ID** is an ID that is used to identify the client on the authorization server.

### Client secret

A **client secret** is a secret that only the client and authorization server know. This allows them to securely share information privately behind the scenes.

### Redirect URI

A **redirect URI** or **callback URL** is a URL that the authorization server will use to redirect the user back to after granting permission to the client.

### Response type

A **response type** is a type of information that client expects to receive from authorization server. The most common response type is *authorization code*.

### Authorization code

An **authorization code** is a short-lived temporary code that the client gives to the authorization server in exchange for an *access token*.
