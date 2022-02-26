# OAuth Grant Flows

## Table of contents

- [OAuth Grant Flows](#oauth-grant-flows)
  - [Table of contents](#table-of-contents)
  - [Authorization code flow](#authorization-code-flow)
  - [Implicit flow](#implicit-flow)
  - [Client credentials flow](#client-credentials-flow)
  - [Resource owner password credentials flow](#resource-owner-password-credentials-flow)
  - [On-behalf-of flow](#on-behalf-of-flow)
  - [Links](#links)

## Authorization code flow

The authorization code grant is used to perform authentication and authorization in the majority of app types, including web apps and natively installed apps.

## Implicit flow

The implicit flow is used by by single page applications: AngularJS, Ember.js, React.js, and so on.

Its primary benefit is that it allows the app to get tokens from authorization server without performing a backend server credential exchange. This allows the app to sign in the user, maintain session, and get tokens to other web APIs within the client JavaScript code.

> There are a few important security considerations to take into account when using the implicit flow specifically around client.

## Client credentials flow

OAuth: used for machine-to-machine communication.

## Resource owner password credentials flow

Resource owner password credential grant allows an application to sign in the user by directly handling their password. The flow requires a high degree of trust and user exposure and you should only use this flow when other, more secure, flows can't be used.

The flow is a single request—it sends the client identification and user's credentials to the IDP, and then receives tokens in return. The client must request username and password before doing so. Immediately after a successful request, the client should securely release the user's credentials from memory. It must never save them.

## On-behalf-of flow

The OAuth 2.0 On-Behalf-Of flow (OBO) serves the use case where an application invokes a service/web API, which in turn needs to call another service/web API. The idea is to propagate the delegated user identity and permissions through the request chain. For the middle-tier service to make authenticated requests to the downstream service, it needs to secure an access token from the AD FS, on behalf of the user.

## Links

- [AD FS OpenID Connect/OAuth flows and Application Scenarios](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/overview/ad-fs-openid-connect-oauth-flows-scenarios)
- [Which OAuth 2.0 Flow Should I Use?](https://auth0.com/docs/get-started/authentication-and-authorization-flow/which-oauth-2-0-flow-should-i-use)
- [An Illustrated Guide to OAuth and OpenID Connect](https://developer.okta.com/blog/2019/10/21/illustrated-guide-to-oauth-and-oidc)
