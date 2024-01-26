# JSON Web Token, JWT

**JSON web token** or **JWT**, pronounced "jot", is an open standard [↑ RFC 7519](https://datatracker.ietf.org/doc/html/rfc7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. Again, JWT is a standard, meaning that all JWTs are tokens, but not all tokens are JWTs.

Because of its relatively small size, a JWT can be sent through a URL, through a POST parameter, or inside an HTTP header, and it is transmitted quickly. A JWT contains all the required information about an entity to avoid querying a database more than once. The recipient of a JWT also does not need to call a server to validate the token.

[↑ jwt.io](https://jwt.io).

## Table of contents

- [JSON Web Token, JWT](#json-web-token-jwt)
  - [Table of contents](#table-of-contents)
  - [Token structure](#token-structure)
  - [Signing algorithms](#signing-algorithms)
  - [JSON Web Key, JWK](#json-web-key-jwk)
  - [JSON Web Key Set, JWKS](#json-web-key-set-jwks)
    - [Locate JSON Web Key Sets](#locate-json-web-key-sets)
    - [Easy Management of Keys with JWK Sets](#easy-management-of-keys-with-jwk-sets)
    - [Simplified Key Rotation Process Using JWKs](#simplified-key-rotation-process-using-jwks)

## Token structure

JWTs have JSON Web Signatures or JWSs, meaning they are signed rather than encrypted. A JWS represents content secured with digital signatures or Message Authentication Codes, MACs, using JSON-based data structures.

A well-formed JWT consists of three concatenated Base64 url-encoded strings, separated by dots:

- **JOSE Header** is a JSON Object Signing and Encryption. It contains metadata about the type of token and the cryptographic algorithms used to secure its contents
- **JWS payload** is a set of claims. It contains verifiable security statements, such as the identity of the user and the permissions they are allowed
- **JWS signature** is used to validate that the token is trustworthy and has not been tampered with. When you use a JWT, you must check its signature before storing and using it

## Signing algorithms

A signature is part of a JWT and is used to verify that the sender of the token is who it says it is and to ensure that the message wasn't changed along the way.

Some algorithms:

- **RS256**, RSA Signature with SHA-256, is an asymmetric algorithm, which means that there are two keys: one public key and one private key that must be kept secret. Authentication server has the private key used to generate the signature, and the consumer of the JWT retrieves a public key from the metadata endpoints provided by authentication server and uses it to validate the JWT signature
- **HS256**, HMAC with SHA-256, is a symmetric algorithm, which means that there is only one private key that must be kept secret, and it is shared between the two parties. Since the same key is used both to generate the signature and to validate it, care must be taken to ensure that the key is not compromised. This private key, or secret, is created when you register your Application, Client Secret or API Signing Secret, and choose the HS256 signing algorithm

## JSON Web Key, JWK

A **JWK** is a JSON object data structure that represents a cryptographic key. It contains the key type, key operations, algorithm, and all relevant material to reconstruct the signature verification key.

The JSON data structure of the JWK format allows for easy, web-native exchange of public keys. The representation of an Elliptic Curve signature key of curve P-256 could look like this:

```json
{
  "kty": "EC",
  "use": "sig",
  "crv": "P-256",
  "kid": "01H1SG7BX197N040C0MHTDV1HR",
  "x": "SEfcECpwqQg-vZ6Lv99RyV0Qkatngz1RV25nI5SOrPg",
  "y": "YU2PRA6pXZ62OW_XuzjJqplqmBUtBwT2pKUZUVxUYfc",
  "alg": "ES256"
}
```

## JSON Web Key Set, JWKS

A **JWKS** is a JSON object that represents a set of JWKs. It must contain a "keys" member and an array of JWKs. JSON Web Key Sets enable identity providers to support and expose more than one public key under a well-known resource. The following example JWK Set format contains two public cryptographic keys – one using an Elliptic Curve (EC) algorithm and a second one using an RSA algorithm:

```json
{
  "keys": [
    {
      "kty": "EC",
      "use": "sig",
      "crv": "P-256",
      "kid": "01H1SG7BX197N040C0MHTDV1HR",
      "x": "SEfcECpwqQg-vZ6Lv99RyV0Qkatngz1RV25nI5SOrPg",
      "y": "YU2PRA6pXZ62OW_XuzjJqplqmBUtBwT2pKUZUVxUYfc",
      "alg": "ES256"
    },
    {
      "kty": "RSA",
      "n": "1qrQCTst3RF04aMC9Ye_kGbsE0sftL4FOtB_WrzBDOFdrfVwLfflQuPX5kJ-0iYv9r2mjD5YIDy8b-iJKwevb69ISeoOrmL3tj6MStJesbbRRLVyFIm_6L7alHhZVyqHQtMKX7IaNndrfebnLReGntuNk76XCFxBBnRaIzAWnzr3WN4UPBt84A0KF74pei17dlqHZJ2HB2CsYbE9Ort8m7Vf6hwxYzFtCvMCnZil0fCtk2OQ73l6egcvYO65DkAJibFsC9xAgZaF-9GYRlSjMPd0SMQ8yU9i3W7beT00Xw6C0FYA9JAYaGaOvbT87l_6ZkAksOMuvIPD_jNVfTCPLQ",
      "e": "AQAB",
      "alg": "RS256",
      "kid": "01H1SGVCX7GKBGE2J2QMQREAGN"
    }
  ]
}
```

### Locate JSON Web Key Sets

Many modern identity providers like Google expose their JWK Sets via the `jwks_uri` attribute within the well-known discovery endpoint of OpenID Connect (OIDC) located at `https://{identity-provider}/.well-known/openid-configuration` which often points to `https://{identity-provider}/.well-known/jwks.json` or a similar resource.

### Easy Management of Keys with JWK Sets

One of the main benefits of using JWKs is that they provide an easy way to manage and distribute keys across multiple servers or locations. Instead of hardcoding public keys into application code, token issuers (authorization servers) only need to provide their clients with an URL for their JSON Web Key Set.

Any application that needs access to the keys can retrieve them from that remote location. This eliminates the need for manual key distribution, resolves, and ensures all applications use the same set of keys. This makes it easier to manage multiple keys and rotate them when necessary.

### Simplified Key Rotation Process Using JWKs

Rotating keys is essential for maintaining security in any system that uses cryptography. However, manually replacing these keys can take time and effort. JWKS can simplify this process by automating key rotation.

As JWKs decouple the authorization server from relying party applications, token issuers can generate new asymmetric key pairs whenever necessary. Setting up regular intervals for key rotation, publishing the new public key material, and using the latest private key automatically after some grace period, can ensure that the amount of keys signed with a particular key has a natural upper limit.

[↑ Mastering JWKS: JSON Web Key Sets Explained](https://www.jbspeakr.cc/jwks-json-web-key-set/).
