# Tokens

## Table of contents

- [Tokens](#tokens)
  - [Table of contents](#table-of-contents)
  - [Tokens vs. Cookies](#tokens-vs-cookies)
  - [JSON Web Token (JWT)](#json-web-token-jwt)
    - [Token structure](#token-structure)
    - [Signing algorithms](#signing-algorithms)
  - [Refresh token](#refresh-token)
  - [Access token](#access-token)
  - [ID token](#id-token)

## Tokens vs. Cookies

Web apps are typically single-page apps (such as Angular, Ember, and Backbone) or native mobile apps (such as iOS, and Android). Web apps consume APIs (written in Node, Ruby, ASP.NET, or a mix of those) and benefit from token-based authentication. Web APIs are traditional server-side applications that use cookie-based authentication.

Token-based authentication is implemented by generating a token when the user authenticates and then setting that token in the `Authorization` header of each subsequent request to your API. You want that token to be something standard, like JSON web tokens since you will find libraries in most of the platforms and you don't want to do your own crypto.

By default, you can use `scope=openid` in token-based authentication to avoid having a huge token. You can control any standard OpenID Connect (OIDC) claims that you want to get in the token by adding them as `scope` values. For example, `scope=openid name email family_name address phone_number`.

## JSON Web Token (JWT)

**JSON web token** or **JWT**, pronounced "jot", is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. Again, JWT is a standard, meaning that all JWTs are tokens, but not all tokens are JWTs.

Because of its relatively small size, a JWT can be sent through a URL, through a POST parameter, or inside an HTTP header, and it is transmitted quickly. A JWT contains all the required information about an entity to avoid querying a database more than once. The recipient of a JWT also does not need to call a server to validate the token.

### Token structure

JWTs have JSON Web Signatures or JWSs, meaning they are signed rather than encrypted. A JWS represents content secured with digital signatures or Message Authentication Codes (MACs) using JSON-based data structures.

A well-formed JWT consists of three concatenated Base64url-encoded strings, separated by dots (.):

**JOSE Header** (JSON Object Signing and Encryption): contains metadata about the type of token and the cryptographic algorithms used to secure its contents.

**JWS payload** (set of claims): contains verifiable security statements, such as the identity of the user and the permissions they are allowed.

**JWS signature**: used to validate that the token is trustworthy and has not been tampered with. When you use a JWT, you must check its signature before storing and using it.

### Signing algorithms

The algorithm used to sign tokens issued for your application or API. A signature is part of a JWT and is used to verify that the sender of the token is who it says it is and to ensure that the message wasn't changed along the way.

Few algorithms:

- **RS256** (RSA Signature with SHA-256) is an asymmetric algorithm, which means that there are two keys: one public key and one private key that must be kept secret. Authentication server has the private key used to generate the signature, and the consumer of the JWT retrieves a public key from the metadata endpoints provided by authentication server and uses it to validate the JWT signature.
- **HS256** (HMAC with SHA-256) is a symmetric algorithm, which means that there is only one private key that must be kept secret, and it is shared between the two parties. Since the same key is used both to generate the signature and to validate it, care must be taken to ensure that the key is not compromised. This private key (or secret) is created when you register your Application (Client Secret) or API (Signing Secret) and choose the HS256 signing algorithm.

## Refresh token

A **refresh token** is a special kind of token used to obtain a renewed *access token*. You can request new access tokens until the refresh token is on the deny list. Applications must store refresh tokens securely because they essentially allow a user to remain authenticated forever.

You can revoke refresh tokens in case they become compromised.

You can also use refresh token rotation so that every time a client exchanges a refresh token to get a new access token, a new refresh token is also returned. Therefore, you no longer have a long-lived refresh token that, if compromised, could provide illegitimate access to resources. As refresh tokens are continually exchanged and invalidated, the threat is reduced.

## Access token

An **access token** is a key that the client will use to communicate with the resource server.

Access tokens do not have to be in any particular format, and in practice, various OAuth servers have chosen many different formats for their access tokens.

You should only ask for a new access token if the access token has expired or you want to refresh the claims contained in the ID token. For example, it's bad practice to call the endpoint to get a new access token every time you call an API.

## ID token

An **ID token** is a token that is used in token-based authentication to cache user profile information and provide it to a client application, thereby providing better performance and experience. The application receives an ID token after a user successfully authenticates, then consumes the ID token and extracts user information from it, which it can then use to personalize the user's experience.

> ID tokens should never be used to obtain direct access to APIs or to make authorization decisions.