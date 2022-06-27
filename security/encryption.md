# Encryption

An **encryption** is the process of scrambling data by use of a *cryptographic key*.

A **cryptographic key** is a string of characters used within an encryption algorithm for altering data so that it appears random. Like a physical key, it locks (encrypts) data so that only someone with the right key can unlock (decrypt) it.

A **plaintext** is the original data that needs to be encrypted.

A **ciphertext** is the plaintext after the key encrypts it.

There are two kinds of encryption: **symmetric encryption** and **asymmetric encryption**, also known as **public key encryption**. In symmetric encryption, both sides of a conversation use the same key for turning plaintext into ciphertext and vice versa.

In asymmetric or public key encryption, the two sides of the conversation each use a different key. One key is called the **public key**, and one key is called the **private key** — thusly named because one of the parties keeps it secret and never shares it with anyone. When plaintext is encrypted with the public key, only the private key can decrypt it, not the public key. It works the other way, too: when plaintext is encrypted with the private key, only the public key can decrypt it.
