# Secure Shell (SSH)

The **Secure Shell** (**SSH**) is a cryptographic network protocol for operating network services securely over an unsecured network.

## Basic Syntax

```bash
ssh remote_host
```

The *remote_host* in this example is the IP address or domain name that you are trying to connect to.

This command assumes that your username on the remote system is the same as your username on your local system.

If your username is different on the remote system, you can specify it by using this syntax:

```bash
ssh remote_username@remote_host
```

To exit back into your local session, simply type:

```bash
exit
```

## SSH Keys

List existing SSH keys:

```bash
ls -al ~/.ssh
```

Generate a new key pair:

```bash
ssh-keygen -t ed25519
```

## Alias in macOS

```bash
echo "alias mysite='ssh mywebsite.abcd'" >> ~/.zshrc
```

## Known Hosts

Open known_hosts file:

```bash
vim ~/.ssh/known_hosts
```

## See Also

* [↑ Best way to use multiple SSH private keys on one client](https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client)
* [↑ Что записано в файле .ssh/known_hosts](https://habr.com/ru/post/421477/)
