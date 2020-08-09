# Secure Shell (SSH)

The **Secure Shell** (**SSH**) is a cryptographic network protocol for operating network services securely over an unsecured network.

## Basic syntax

```console
ssh remote_host
```

The *remote_host* in this example is the IP address or domain name that you are trying to connect to.

This command assumes that your username on the remote system is the same as your username on your local system.

If your username is different on the remote system, you can specify it by using this syntax:

```console
ssh remote_username@remote_host
```

To exit back into your local session, simply type:

```console
exit
```

## SSH Keys

List existing SSH keys:

```console
ls -al ~/.ssh
```
