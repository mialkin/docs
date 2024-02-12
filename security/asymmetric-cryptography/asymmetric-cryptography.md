# Asymmetric cryptography

## Public key certificate

A **public key certificate**, also known as a **digital certificate** or **identity certificate**, is an electronic document used to prove the ownership of a public key.

The certificate includes information about the key, information about the identity of its owner, called the **subject**, and the digital signature of an entity that has verified the certificate's contents, called the **issuer**. If the signature is valid, and the software examining the certificate trusts the issuer, then it can use that key to communicate securely with the certificate's subject.

In a typical public-key infrastructure, PKI, scheme, the certificate issuer is a certificate authority, CA, usually a company that charges customers to issue certificates for them.

The most common format for public key certificates is defined by [↑ X.509](https://en.wikipedia.org/wiki/X.509) standard.

## Public key fingerprint

In public-key cryptography, a **public key fingerprint** is a short sequence of bytes used to identify a longer public key. Fingerprints are created by applying a cryptographic hash function to a public key. Since fingerprints are shorter than the keys they refer to, they can be used to simplify certain key management tasks.

A public key fingerprint is typically created through the following steps:

* A public key (and optionally some additional data) is encoded into a sequence of bytes.

* If desired, the hash function output can be truncated to provide a shorter, more convenient fingerprint.

* This process produces a short fingerprint which can be used to authenticate a much larger public key. For example, whereas a typical RSA public key will be 1024 bits in length or longer, typical MD5 or SHA-1 fingerprints are only 128 or 160 bits in length.

When displayed for human inspection, fingerprints are usually encoded into hexadecimal strings.

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
