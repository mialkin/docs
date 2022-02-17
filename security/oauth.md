# OAuth

**OAuth** (**O**pen **A**uthorization) is the industry-standard protocol for authorization.

OAuth is used for granting websites or applications access to subset of user's information stored on another website.

OAuth is directly related to OpenID Connect (OIDC), since OIDC is an authentication layer built on top of OAuth 2.0.

- [OAuth](#oauth)
  - [Terminology](#terminology)
    - [Client](#client)
    - [User](#user)
    - [Authorization server](#authorization-server)
    - [Resoure server](#resoure-server)
    - [Scope](#scope)
    - [Consent](#consent)
    - [Client ID](#client-id)
    - [Redirect URI](#redirect-uri)
    - [Access token](#access-token)

## Terminology

### Client

A **client** or an **app** or a **third-party app** is an application that uses OAuth to access some data or perform some actions on the *resource server* on behalf of the *user*.

### User

A **user** or a **resource owner** is an identity on behalf of which client wants to access some data or perform some actions on the *resource server*.

### Authorization server

An **authorization server** is a service that that knows the user, where the user already has an account.

### Resoure server

A **resoure server** is an API or service that client wants to use on behalf of the user.

### Scope

A **scope** is a set of granular permissions that client wants to do on resource server, such as have access to data or to perform actions.

### Consent

A consent is user's agreement to grant client permissions specified in the scope.

The authorization server takes the scopes the client is requesting, and verifies with the user whether or not he wants to give the client permission.

### Client ID

A **client ID** is an ID is used to identify the client within the authorization server.

### Redirect URI

A **redirect URI**: The URL the Authorization Server will redirect the Resource Owner back to after granting permission to the Client. This is sometimes referred to as the “Callback URL.”

### Access token

An **access token** is a string that the client uses to make requests to the resource server.

Access tokens do not have to be in any particular format, and in practice, various OAuth servers have chosen many different formats for their access tokens.