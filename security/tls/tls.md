# TLS
## Table of contents

- [TLS](#tls)
  - [Table of contents](#table-of-contents)
  - [TLS](#tls-1)
    - [Public key and private key](#public-key-and-private-key)
    - [TLS certificate](#tls-certificate)
    - [TLS handshake](#tls-handshake)
    - [How are keys used in TLS encryption](#how-are-keys-used-in-tls-encryption)
    - [Understanding cipher suite](#understanding-cipher-suite)

## TLS

The **Transport Layer Security** or **TLS** is an encryption protocol in wide use on the Internet. TLS, which was formerly called SSL, authenticates the server in a client-server connection and encrypts communications between client and server so that external parties cannot spy on the communications.

There are three important things to understand about how TLS works:

- Public key and private key
- TLS certificate
- TLS handshake

### Public key and private key

TLS works using a technique called *public key encryption*, which relies on a pair of keys — a *public key* and a *private key*.

- Anything encrypted with the public key can be decrypted only with the private key
- Anything encrypted with the private key can be decrypted only with the public key

Therefore, a server that decrypts a message that was encrypted with the public key proves that it possesses the private key. Anyone can view the public key by looking at the domain's or server's TLS certificate.

### TLS certificate

A **TLS certificate** is a data file that contains important information for verifying a server's or device's identity, including the public key, a statement of who issued the certificate (TLS certificates are issued by a certificate authority), and the certificate's expiration date.

### TLS handshake

The **TLS handshake** is the process for verifying the TLS certificate and the server's possession of the private key. The TLS handshake also establishes how encryption will take place once the handshake is finished.

### How are keys used in TLS encryption

TLS is an encryption protocol used to keep Internet communications secure, and a website that is served over HTTPS instead of HTTP uses this kind of encryption. In TLS a website or web application will have both a public key and a private key. The public key is shared publicly in the website's TLS certificate for anyone to see. The private key is installed on the origin server and never shared.

TLS *communication sessions* begin with a *TLS handshake*, during which the website and the client use the public key and the private key in order to generate new keys, which are called *session keys*. These session keys are then used by both sides to encrypt their messages back and forth.

Thus, TLS starts with asymmetric encryption (with two keys) and moves to symmetric encryption (with one key). Both sides use the same keys during the communication session, but when they start a new session, they will generate new keys together.

A **session key** is any encryption key used to symmetrically encrypt one communication session only. In other words, it's a temporary key that is only used once, during one stretch of time, for encrypting and decrypting data; future conversations between the two parties would be encrypted with different session keys. A session key is like a password that someone resets every time they log in.

In TLS, the two communicating parties (the client and the server) generate 4 session keys at the start of any communication session, during the TLS handshake. The official [↑ RFC for TLS](https://datatracker.ietf.org/doc/html/rfc5246) does not actually call these keys "session keys", but functionally that's exactly what they are.

### Understanding cipher suite

We gonna look at TLS 1.2 handshake, not 1.3, because 1.2 is more intuitive when you learn it for the first time.

TLS connection sits over TCP and is encrypting HTTP before it gets sent over.

Take a look at the certificate of the https://www.nottingham.ac.uk website inside Firefox browser:

<img src="cipher suite.png" alt="drawing" width="700"/>

 You'll find there following string:

```text
TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256, 128 bit keys, TLS 1.2
```

Above is a [↑ cipher suite](https://en.wikipedia.org/wiki/Cipher_suite), i.e. a string representation of all the things we are going to do during handshake:

* **TLS** — [↑ Transport Layer Security](https://en.wikipedia.org/wiki/Transport_Layer_Security).
* **ECDHE** — [↑ Elliptic-curve Diffie–Hellman](https://en.wikipedia.org/wiki/Elliptic-curve_Diffie–Hellman) key exchange. This is how we obtain a shared secret — the one which only you and I have, or only me and the web server has, so nobody else can see what we are doing.
* **RSA** — [↑ RSA (Rivest–Shamir–Adleman) cryptosystem](https://en.wikipedia.org/wiki/RSA_(cryptosystem)) public-key authentication mechanism. We are going to make sure that this service is who he is by checking public key and verifying signature.
* **AES** — [↑ Advanced Encryption Standard (AES)](https://en.wikipedia.org/wiki/Advanced_Encryption_Standard) cipher and **128** is the key size.
* **GCM** — [↑ Galois/Counter Mode (GCM)](https://ru.wikipedia.org/wiki/Galois/Counter_Mode) mode of operation.
* **SHA256** — [↑ SHA-2 (Secure Hash Algorithm 2)](https://en.wikipedia.org/wiki/SHA-2) building block to perform hash functions where they are needed.

The handshake is **TLS_ECDHE_RSA**. When we are done with handshake part, then the actual ecryption is going to be with AES.

Here is an example of another cipher suite:

<img src="another cipher suite.png" alt="drawing" width="600"/>
