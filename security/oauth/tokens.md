# Authentication tokens, JWT

An **authentication token** is a computer-generated code that verifies a user’s identity.

Auth tokens are used to access websites, applications, services, and APIs.

## Table of contents

- [Authentication tokens, JWT](#authentication-tokens-jwt)
  - [Table of contents](#table-of-contents)
  - [Token types](#token-types)
    - [Refresh token](#refresh-token)
    - [Access token](#access-token)
    - [ID token](#id-token)
  - [JWT, JSON Web Token](#jwt-json-web-token)
    - [Token structure](#token-structure)
    - [Signing algorithms](#signing-algorithms)
    - [Keep the timeout short](#keep-the-timeout-short)

## Token types

### Refresh token

A **refresh token** is a special kind of token used to obtain a renewed *access token*. You can request new access tokens until the refresh token is on the deny list. Applications must store refresh tokens securely because they essentially allow a user to remain authenticated forever.

You can revoke refresh tokens in case they become compromised.

You can also use refresh token rotation so that every time a client exchanges a refresh token to get a new access token, a new refresh token is also returned. Therefore, you no longer have a long-lived refresh token that, if compromised, could provide illegitimate access to resources. As refresh tokens are continually exchanged and invalidated, the threat is reduced.

### Access token

An **access token** is a key that the client will use to communicate with the resource server.

Access tokens do not have to be in any particular format, and in practice, various OAuth servers have chosen many different formats for their access tokens.

You should only ask for a new access token if the access token has expired or you want to refresh the claims contained in the ID token. For example, it's bad practice to call the endpoint to get a new access token every time you call an API.

### ID token

An **ID token** is a token that is used in token-based authentication to cache user profile information and provide it to a client application, thereby providing better performance and experience. The application receives an ID token after a user successfully authenticates, then consumes the ID token and extracts user information from it, which it can then use to personalize the user's experience.

ID tokens should never be used to obtain direct access to APIs or to make authorization decisions.

## JWT, JSON Web Token

**JSON web token** or **JWT**, pronounced "jot", is an open standard [↑ RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. Again, JWT is a standard, meaning that all JWTs are tokens, but not all tokens are JWTs.

Because of its relatively small size, a JWT can be sent through a URL, through a POST parameter, or inside an HTTP header, and it is transmitted quickly. A JWT contains all the required information about an entity to avoid querying a database more than once. The recipient of a JWT also does not need to call a server to validate the token.

[↑ jwt.io](https://jwt.io).

### Token structure

JWTs have JSON Web Signatures or JWSs, meaning they are signed rather than encrypted. A JWS represents content secured with digital signatures or Message Authentication Codes, MACs, using JSON-based data structures.

A well-formed JWT consists of three concatenated Base64 url-encoded strings, separated by dots:

- **JOSE Header** is a JSON Object Signing and Encryption. It contains metadata about the type of token and the cryptographic algorithms used to secure its contents
- **JWS payload** is a set of claims. It contains verifiable security statements, such as the identity of the user and the permissions they are allowed
- **JWS signature** is used to validate that the token is trustworthy and has not been tampered with. When you use a JWT, you must check its signature before storing and using it

### Signing algorithms

A signature is part of a JWT and is used to verify that the sender of the token is who it says it is and to ensure that the message wasn't changed along the way.

Some algorithms:

- **RS256**, RSA Signature with SHA-256, is an asymmetric algorithm, which means that there are two keys: one public key and one private key that must be kept secret. Authentication server has the private key used to generate the signature, and the consumer of the JWT retrieves a public key from the metadata endpoints provided by authentication server and uses it to validate the JWT signature
- **HS256**, HMAC with SHA-256, is a symmetric algorithm, which means that there is only one private key that must be kept secret, and it is shared between the two parties. Since the same key is used both to generate the signature and to validate it, care must be taken to ensure that the key is not compromised. This private key, or secret, is created when you register your Application, Client Secret or API Signing Secret, and choose the HS256 signing algorithm

### Keep the timeout short

If you are concerned about token lifetime, the key is to keep the token timeout short. This means tokens have a limited lifespan and are unlikely to be useful to anything but live attacks.

Timeouts can be a pain for client applications since they now have to check for the token timeouts. You can ease that pain by having returning useful error information on a timeout instead of just a 401, and have an easy way to create a new token so that client applications can automate that process.
