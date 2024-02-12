# Asymmetric cryptography

**Public-key cryptography**, or **asymmetric cryptography**, is the field of cryptographic systems that use pairs of related keys. Each key pair consists of a **public key** and a corresponding **private key**.

Key pairs are generated with cryptographic algorithms based on mathematical problems termed [↑ one-way functions](https://en.wikipedia.org/wiki/One-way_function).

Security of public-key cryptography depends on keeping the private key secret; the public key can be openly distributed without compromising security

## Public key certificate

A **public key certificate**, also known as a **digital certificate** or **identity certificate**, is an electronic document used to prove the ownership of a public key.

The certificate includes information about the key, information about the identity of its owner, called the **subject**, and the digital signature of an entity that has verified the certificate's contents, called the **issuer**. If the signature is valid, and the software examining the certificate trusts the issuer, then it can use that key to communicate securely with the certificate's subject.

In a typical public-key infrastructure, PKI, scheme, the certificate issuer is a certificate authority, CA, usually a company that charges customers to issue certificates for them.

The most common format for public key certificates is defined by [↑ X.509](https://en.wikipedia.org/wiki/X.509) standard.

## Public key fingerprint

In public-key cryptography, a **public key fingerprint** is a short sequence of bytes used to identify a longer public key. Fingerprints are created by applying a cryptographic hash function to a public key. Since fingerprints are shorter than the keys they refer to, they can be used to simplify certain key management tasks.

A public key fingerprint is typically created through the following steps:

* A public key, and optionally some additional data, is encoded into a sequence of bytes.

* If desired, the hash function output can be truncated to provide a shorter, more convenient fingerprint.

* This process produces a short fingerprint which can be used to authenticate a much larger public key. For example, whereas a typical RSA public key will be 1024 bits in length or longer, typical MD5 or SHA-1 fingerprints are only 128 or 160 bits in length.

When displayed for human inspection, fingerprints are usually encoded into hexadecimal strings.

### Using public key fingerprints for key authentication

When a public key is received over an untrusted channel, such as the Internet, the recipient often wishes to authenticate the public key. Fingerprints can help accomplish this, since their small size allows them to be passed over trusted channels where public keys won't easily fit.

For example, if Alice wishes to authenticate a public key as belonging to Bob, she can contact Bob over the phone or in person and ask him to read his fingerprint to her, or give her a scrap of paper with the fingerprint written down. Alice can then check that this trusted fingerprint matches the fingerprint of the public key. Exchanging and comparing values like this is much easier if the values are short fingerprints instead of long public keys.

Fingerprints can also be useful when automating the exchange or storage of key authentication data. For example, if key authentication data needs to be transmitted through a protocol or stored in a database where the size of a full public key is a problem, then exchanging or storing fingerprints may be a more viable solution.

In addition, fingerprints can be queried with search engines in order to ensure that the public key that a user just downloaded can be seen by third party search engines. If the search engine returns hits referencing the fingerprint linked to the proper site(s), one can feel more confident that the key is not being injected by an attacker, such as a Man-in-the-middle attack.

[↑ Public key fingerprint](https://en.wikipedia.org/wiki/Public_key_fingerprint).

## Digital signature

A **digital signature** is a mathematical scheme for verifying the authenticity of digital messages or documents. A valid digital signature, where the prerequisites are satisfied, gives a recipient very strong reason to believe that the message was created by a known sender, and that the message was not altered in transit.

Digital signatures can also provide non-repudiation, meaning that the signer cannot successfully claim they did not sign a message, while also claiming their private key remains secret. Further, some non-repudiation schemes offer a time stamp for the digital signature, so that even if the private key is exposed, the signature is valid.

A digital signature scheme typically consists of three algorithms:

* A key generation algorithm that selects a private key uniformly at random from a set of possible private keys. The algorithm outputs the private key and a corresponding public key.

* A signing algorithm that, given a message and a private key, produces a signature.

* A signature verifying algorithm that, given the message, public key and signature, either accepts or rejects the message's claim to authenticity.

Two main properties are required. First, the authenticity of a signature generated from a fixed message and fixed private key can be verified by using the corresponding public key. Secondly, it should be computationally infeasible to generate a valid signature for a party without knowing that party's private key.

<div align="center">
  <a href="https://www.youtube.com/watch?v=stsWa9A3sOM"><img src="digital-signature.png" alt="IMAGE ALT TEXT"></a>
</div>
