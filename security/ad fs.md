# Active Directory Federation Services

**Active Directory Federation Services** or **AD FS**, is a software component developed by Microsoft, which can run on Windows Server operating systems to provide users with *single sign-on* access to systems and applications located across organizational boundaries. It uses a claims-based access-control authorization model to maintain application security and to implement *federated identity*. Claims-based authentication involves authenticating a user based on a set of claims about that user's identity contained in a trusted token. Such a token is often issued and signed by an entity that is able to authenticate the user by other means, and that is trusted by the entity doing the claims-based authentication. It is part of the Active Directory Services.

## Single Sign-on

A **single sign-on** or **SSO** is an authentication scheme that allows a user to log in with a single ID to any of several related, yet independent, software systems. SSO allows users to authenticate once and access multiple resources without being prompted for additional credentials. It should not be confused with same-sign on, often accomplished by using the Lightweight Directory Access Protocol or LDAP and stored in LDAP databases on (directory) servers.

AD FS supports several types of SSO experiences:

- **Session SSO**: cookies are written for the authenticated user which eliminates further prompts when the user switches applications during a particular session. However, if a particular session ends, the user will be prompted for their credentials again.
- **Persistent SSO**: cookies are written for the authenticated user which eliminates further prompts when the user switches applications for as long as the persistent SSO cookie is valid. The difference between persistent SSO and session SSO is that persistent SSO can be maintained across different sessions.
- **Application specific SSO**: in the OAuth scenario, a refresh token is used to maintain the SSO state of the user within the scope of a particular application.

## Federated Identity

A **federated identity** is the means of linking a person's electronic identity and attributes, stored across multiple distinct identity management systems.

Federated identity is related to *single sign-on* or *SSO*, in which a user's single authentication ticket, or token, is trusted across multiple IT systems or even organizations. SSO is a subset of federated identity management, as it relates only to authentication and is understood on the level of technical interoperability and it would not be possible without some sort of federation.
