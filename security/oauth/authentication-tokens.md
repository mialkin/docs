# Authentication tokens

An **authentication token** is a computer-generated code that verifies a user’s identity.

Auth tokens are used to access websites, applications, services, and APIs.

## Table of contents

- [Authentication tokens](#authentication-tokens)
  - [Table of contents](#table-of-contents)
  - [Token types](#token-types)
    - [Refresh token](#refresh-token)
    - [Access token](#access-token)
    - [ID token](#id-token)

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
