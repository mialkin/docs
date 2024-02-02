# OAuth

**OAuth** 2.0, **O**pen **A**uthorization, is the industry-standard protocol for authorization.

OAuth is used for granting websites or applications access to subset of user's information stored on another website.

OAuth is directly related to OpenID Connect, OIDC, since OIDC is an authentication layer built on top of OAuth 2.0.

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
  - [OAuth Grant Flows](#oauth-grant-flows)
    - [Authorization code flow](#authorization-code-flow)
    - [Implicit flow](#implicit-flow)
    - [Client credentials flow](#client-credentials-flow)
    - [Resource owner password flow](#resource-owner-password-flow)
  - [OpenID Connect](#openid-connect)
    - [OpenID vs OAuth2](#openid-vs-oauth2)
    - [OpenID and JWTs](#openid-and-jwts)
  - [Authentication tokens](#authentication-tokens)
    - [Token types](#token-types)
      - [Refresh token](#refresh-token)
      - [Access token](#access-token)
      - [ID token](#id-token)

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

## OAuth Grant Flows

There are several common flows below, but not all of them:

### Authorization code flow

The authorization code grant is used to perform authentication and authorization in the majority of app types, including web apps and natively installed apps.

### Implicit flow

The implicit flow is used by by single page applications: AngularJS, Ember.js, React.js, and so on.

Its primary benefit is that it allows the app to get tokens from authorization server without performing a backend server credential exchange. This allows the app to sign in the user, maintain session, and get tokens to other web APIs within the client JavaScript code.

> There are a few important security considerations to take into account when using the implicit flow specifically around client.

### Client credentials flow

The client credentials flow is typically used to access web-hosted resources by using the identity of an application. This type of grant is commonly used for server-to-server interactions that must run in the background, without immediate interaction with a user. These types of applications are often referred to as daemons or service accounts.

This flow permits a web service (confidential client) to use its own credentials, instead of impersonating a user, to authenticate when calling another web service. In this scenario, the client is typically a middle-tier web service, a daemon service, or a web site.

### Resource owner password flow

Resource owner password credential grant allows an application to sign in the user by directly handling their password. The flow requires a high degree of trust and user exposure and you should only use this flow when other, more secure, flows can't be used.

The flow is a single request—it sends the client identification and user's credentials to the IDP, and then receives tokens in return. The client must request username and password before doing so. Immediately after a successful request, the client should securely release the user's credentials from memory. It must never save them.  This flow is generally not recommended.

## OpenID Connect

**OpenID Connect 1.0**, **OIDC 1.0** , is a simple identity layer, an authentication protocol built on top of the OAuth 2.0 protocol, which allows clients to verify the identity of an user based on the authentication performed by an authorization server, as well as to obtain basic profile information about the user in an interoperable and REST-like manner.

OpenID Connect allows clients of all types, including Web-based, mobile, and JavaScript clients, to request and receive information about authenticated sessions and end-users. The specification suite is extensible, allowing participants to use optional features such as encryption of identity data, discovery of OpenID Providers, and session management, when it makes sense for them.

### OpenID vs OAuth2

While OAuth 2.0 is about resource access and sharing, OIDC is about user authentication. Its purpose is to give you one login for multiple sites. Each time you need to log in to a website using OIDC, you are redirected to your OpenID site where you log in, and then taken back to the website. For example, if you chose to sign in to Auth0 using your Google account then you used OIDC. Once you successfully authenticate with Google and authorize Auth0 to access your information, Google sends information back to Auth0 about the user and the authentication performed. This information is returned in a JWT. You'll receive an access token and if requested, an ID token.

### OpenID and JWTs

JWTs contain *claims*, which are statements, such as name or email address, about an entity, typically, the user, and additional metadata. The OpenID Connect specification defines a set of *standard claims*. The set of standard claims include name, email, gender, birth date, and so on. However, if you want to capture information about a user and there currently isn't a standard claim that best reflects this piece of information, you can create custom claims and add them to your tokens.

## Authentication tokens

An **authentication token** is a computer-generated code that verifies a user’s identity.

Auth tokens are used to access websites, applications, services, and APIs.

### Token types

#### Refresh token

A **refresh token** is a special kind of token used to obtain a renewed *access token*. You can request new access tokens until the refresh token is on the deny list. Applications must store refresh tokens securely because they essentially allow a user to remain authenticated forever.

You can revoke refresh tokens in case they become compromised.

You can also use refresh token rotation so that every time a client exchanges a refresh token to get a new access token, a new refresh token is also returned. Therefore, you no longer have a long-lived refresh token that, if compromised, could provide illegitimate access to resources. As refresh tokens are continually exchanged and invalidated, the threat is reduced.

#### Access token

An **access token** is a key that the client will use to communicate with the resource server.

Access tokens do not have to be in any particular format, and in practice, various OAuth servers have chosen many different formats for their access tokens.

You should only ask for a new access token if the access token has expired or you want to refresh the claims contained in the ID token. For example, it's bad practice to call the endpoint to get a new access token every time you call an API.

#### ID token

An **ID token** is a token that is used in token-based authentication to cache user profile information and provide it to a client application, thereby providing better performance and experience. The application receives an ID token after a user successfully authenticates, then consumes the ID token and extracts user information from it, which it can then use to personalize the user's experience.

ID tokens should never be used to obtain direct access to APIs or to make authorization decisions.
