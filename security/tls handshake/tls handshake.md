# TLS handshake

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
