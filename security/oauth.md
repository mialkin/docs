# OAuth

**OAuth** (**O**pen **A**uthorization) is the industry-standard protocol for authorization.

OAuth is used for granting websites and/or applications access to subset of user's information stored on another website.

OAuth is directly related to OpenID Connect (OIDC), since OIDC is an authentication layer built on top of OAuth 2.0.

## Terminology

### Client

**Client** or **app** or **third-party app** is an application that uses OAuth to access some information about *user* on *resource server*.

### User

**User** or **resource owner** is an identity whose information app wants to retrieve from *resource server*.

### Authorization server

**Authorization server** or **token factory** is a service that that knows the user, where the user already has an account. Authorization server issues *access token* to the client.

### Resoure server

**Resoure server** is and API that gives client some resource when client provides valid access token.

### Access token

An OAuth Access Token is a string that the OAuth client uses to make requests to the resource server.

Access tokens do not have to be in any particular format, and in practice, various OAuth servers have chosen many different formats for their access tokens.

### Refresh Token

An OAuth Refresh Token is a string that the OAuth client can use to get a new access token without the user's interaction.

A refresh token must not allow the client to gain any access beyond the scope of the original grant. The refresh token exists to enable authorization servers to use short lifetimes for access tokens without needing to involve the user when the token expires.

### OAuth Scopes

Scope is a mechanism in OAuth 2.0 to limit an application's access to a user's account. An application can request one or more scopes, this information is then presented to the user in the consent screen, and the access token issued to the application will be limited to the scopes granted.

The OAuth spec allows the authorization server or user to modify the scopes granted to the application compared to what is requested, although there are not many examples of services doing this in practice.

OAuth does not define any particular values for scopes, since it is highly dependent on the service's internal architecture and needs.