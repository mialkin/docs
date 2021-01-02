# TLS handshake

We gonna look at TLS 1.2 handshake, not 1.3, because 1.2 is more intuitive when you learn it for the first time.

TLS connection sits over TCP and is encrypting HTTP before it gets sent over.

Take a look at the certificate of the https://www.nottingham.ac.uk website inside Firefox browser. You'll find there following string:

```text
TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256, 128 bit keys, TLS 1.2
```

Above is a "cyper suite", i.e. a string representation of all the things we are going to do during handshake.
