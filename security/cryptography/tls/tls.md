# TLS, mTLS

## Table of contents

- [TLS, mTLS](#tls-mtls)
  - [Table of contents](#table-of-contents)
  - [TLS](#tls)
    - [Public key and private key](#public-key-and-private-key)
    - [TLS certificate](#tls-certificate)
    - [TLS handshake](#tls-handshake)
    - [How are keys used in TLS encryption](#how-are-keys-used-in-tls-encryption)
    - [Session key](#session-key)
    - [Session](#session)
    - [Does HTTPS use symmetric or asymmetric encryption?](#does-https-use-symmetric-or-asymmetric-encryption)
    - [How does a TLS handshake work?](#how-does-a-tls-handshake-work)
    - [4 session keys](#4-session-keys)
    - [Understanding cipher suite](#understanding-cipher-suite)
    - [TLS termination](#tls-termination)
  - [mTLS](#mtls)

## TLS

The **Transport Layer Security** or **TLS** is an encryption protocol in wide use on the Internet.

TLS, which was formerly called SSL, authenticates the server in a client-server connection and encrypts communications between client and server so that external parties cannot spy on the communications.

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

### Session key

A **session key** is any encryption key used to symmetrically encrypt one communication session only. In other words, it's a temporary key that is only used once, during one stretch of time, for encrypting and decrypting data; future conversations between the two parties would be encrypted with different session keys. A session key is like a password that someone resets every time they log in.

In TLS, the two communicating parties (the client and the server) generate 4 session keys at the start of any communication session, during the TLS handshake. The official [↑ RFC for TLS](https://datatracker.ietf.org/doc/html/rfc5246) does not actually call these keys "session keys", but functionally that's exactly what they are.

[↑ Session keys and TLS handshakes](https://www.cloudflare.com/learning/ssl/what-is-a-session-key).

### Session

A **session** is essentially a conversation. A session takes place over a network, and it begins when two devices acknowledge each other and open a virtual connection. It ends when the two devices have obtained the information they need from each other and send "finished" messages, terminating the connection, much like if two people are texting each other, and they close the conversation by saying, "Talk to you later." The connection can also time out due to inactivity, like if two people are texting and simply stop responding to each other.

A session can either be a set period of time, or it can last for as long as the two parties are communicating. If the former, the session will expire after a certain amount of time; in the context of TLS encryption, the two devices would then have to exchange information and generate new session keys to reopen the connection.

### Does HTTPS use symmetric or asymmetric encryption?

HTTPS, which is HTTP with the TLS encryption protocol, uses both types of encryption. All communications over TLS start with a TLS handshake. Asymmetric encryption is crucial for making the TLS handshake work.

During the course of a TLS handshake, the two communicating devices will establish the 4 session keys, and these will be used for symmetric encryption for the rest of the session. Usually, the two communicating devices are a client, or a user device like a laptop or a smartphone, and a server, which is any web server that hosts a website.

### How does a TLS handshake work?

During a TLS handshake, both client and server send each other random data, which they use to make calculations separately and then derive the same session keys. Three kinds of randomly generated data are sent from one side to the other:

- The "client random"
- The "server random"
- The "premaster secret"

The **client random** is a random string of bytes that the client sends to the server.

The **server random** is a random string of bytes that the server sends to the client.

The **premaster secret** is yet another string of data. In some versions of the TLS handshake, the client generates this and sends it to the server encrypted with the public key; in other versions, the client and server generate the premaster secret on their own, using agreed-upon algorithm parameters to arrive at the same result.

The TLS handshake uses asymmetric encryption either to hide the server random from attackers (by encrypting it with a private key), or for allowing the server to digitally "sign" one of its messages so that the client knows the server is who it claims to be (just as a signature helps verify someone's identity in real life). The server encrypts some data with the private key, and the client uses the public key to decrypt it, proving that the server has the correct key and is legitimate.

The "**master secret**" is the final result from combining the client random, the server random, and the premaster secret via an algorithm. The client and the server each have those three messages, so they should arrive at the same result for the master secret.

The client and server then use the master secret to calculate several session keys for use in that session only – 4 session keys, to be precise.

### 4 session keys

The 4 kinds of session keys created in each TLS handshake are:

- The "client write key"
- The "server write key"
- The "client write MAC key"
- The "server write MAC key"

The **client write key** is the key that the client uses to encrypt its messages. The client write key is a symmetric key, and both the client and the server have it. This enables the server to decrypt messages from the client using the same key.

The **server write key** is just like the client write key, except on the server side. To summarize: Messages from client to server are encrypted with the client write key, and the server uses the client write key to decrypt them. Messages from server to client are encrypted with the server write key, and the client uses the server write key to decrypt them.

The **MAC**, or **message authentication code**, **keys** are used to digitally sign messages. The server signs its messages with the server write MAC key, and when the client receives the message, it can check the MAC key used against its own record of the server MAC key to make sure it's legitimate. The client signs its messages with the client write MAC key.

A set of 4 completely new session keys gets created with every new communication session and new TLS handshake.

### Understanding cipher suite

We gonna look at TLS 1.2 handshake, not 1.3, because 1.2 is more intuitive when you learn it for the first time.

TLS connection sits over TCP and is encrypting HTTP before it gets sent over.

Take a look at the certificate of the https://www.nottingham.ac.uk website inside Firefox browser:

<img src="cipher-suite.png" alt="drawing" width="700"/>

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

<img src="another-cipher-suite.png" alt="drawing" width="600"/>

### TLS termination

**TLS termination** is a process of decrypting traffic before it's passed on another server such as gateway API.

When used with a load balancer, TLS can be terminated at the load balancer or encrypted traffic can be passed directly to gateway API and TLS terminated there.

Which method is selected is largely a matter of preference. When TLS is terminated at the load balancer then decisions can be made about the traffic based on the information itself. Sophisticated load balancers provide such functionality. Often its a benefit to the back end server to terminate TLS at the load balancer. For example to conserve CPU performance and then not requiring decryption by the back end.

[↑ Manage SSL/TLS termination](https://help.okta.com/oag/en-us/content/topics/access-gateway/manage-tls-termination.htm).

## mTLS

**Mutual TLS** or **mTLS** is a method for mutual authentication. mTLS ensures that the parties at each end of a network connection are who they claim to be by verifying that they both have the correct private key. The information within their respective TLS certificates provides additional verification.

mTLS is often used in a [↑ Zero Trust security](https://www.cloudflare.com/learning/security/glossary/what-is-zero-trust/) framework to verify users, devices, and servers within an organization. It can also help keep APIs secure.
