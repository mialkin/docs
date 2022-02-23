# JSON Web Token (JWT)

**JSON web token** or **JWT**, pronounced "jot", is an open standard (RFC 7519) that defines a compact and self-contained way for securely transmitting information between parties as a JSON object. Again, JWT is a standard, meaning that all JWTs are tokens, but not all tokens are JWTs.

Because of its relatively small size, a JWT can be sent through a URL, through a POST parameter, or inside an HTTP header, and it is transmitted quickly. A JWT contains all the required information about an entity to avoid querying a database more than once. The recipient of a JWT also does not need to call a server to validate the token.

## Token structure

JWTs have JSON Web Signatures or JWSs, meaning they are signed rather than encrypted. A JWS represents content secured with digital signatures or Message Authentication Codes (MACs) using JSON-based data structures.

A well-formed JWT consists of three concatenated Base64url-encoded strings, separated by dots (.):

**JOSE Header** (JSON Object Signing and Encryption): contains metadata about the type of token and the cryptographic algorithms used to secure its contents.

**JWS payload** (set of claims): contains verifiable security statements, such as the identity of the user and the permissions they are allowed.

**JWS signature**: used to validate that the token is trustworthy and has not been tampered with. When you use a JWT, you must check its signature before storing and using it.

## Signing algorithms

A signature is part of a JWT and is used to verify that the sender of the token is who it says it is and to ensure that the message wasn't changed along the way.

Some algorithms:

- **RS256** (RSA Signature with SHA-256) is an asymmetric algorithm, which means that there are two keys: one public key and one private key that must be kept secret. Authentication server has the private key used to generate the signature, and the consumer of the JWT retrieves a public key from the metadata endpoints provided by authentication server and uses it to validate the JWT signature.
- **HS256** (HMAC with SHA-256) is a symmetric algorithm, which means that there is only one private key that must be kept secret, and it is shared between the two parties. Since the same key is used both to generate the signature and to validate it, care must be taken to ensure that the key is not compromised. This private key (or secret) is created when you register your Application (Client Secret) or API (Signing Secret) and choose the HS256 signing algorithm.