# Secure Shell (SSH)

- [Secure Shell (SSH)](#secure-shell-ssh)
  - [Manage keys](#manage-keys)
    - [List](#list)
    - [Generate](#generate)
    - [Get public key](#get-public-key)
  - [Connect](#connect)
  - [`known_hosts` file](#known_hosts-file)
  - [Links](#links)

The **Secure Shell** (**SSH**) is a cryptographic network protocol for operating network services securely over an unsecured network.

## Manage keys

### List

List existing SSH keys:

```bash
ls -al ~/.ssh
```

### Generate

Generate a new key pair:

```bash
ssh-keygen
# ssh-keygen -t ed25519
```

### Get public key

Copy public key into clipboard:

```bash
cat ~/.ssh/id_rsa.pub | pbcopy
```

## Connect

```bash
ssh remote_host
```

The _remote_host_ in this example is the IP address or domain name that you are trying to connect to.

This command assumes that your username on the remote system is the same as your username on your local system.

If your username is different on the remote system, you can specify it by using this syntax:

```bash
ssh remote_username@remote_host
```

If you have multiple privates keys, specify the one to use:

```bash
ssh -i path_to_private_key_file remote_username@remote_host
```

To exit back into your local session, simply type:

```bash
exit
```

You can create an alias to connect quicker:

```bash
echo "alias mysite='ssh mywebsite.abcd'" >> ~/.zshrc
```

## `known_hosts` file

Open known_hosts file:

```bash
vim ~/.ssh/known_hosts
```

## Links

- [↑ Best way to use multiple SSH private keys on one client](https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client)
- [↑ Что записано в файле .ssh/known_hosts](https://habr.com/ru/post/421477/)
- [↑ How to Set Up SSH Keys on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04)
