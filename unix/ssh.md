# Secure Shell (SSH)

- [Secure Shell (SSH)](#secure-shell-ssh)
  - [On client](#on-client)
    - [List existing keys](#list-existing-keys)
    - [Generate new key](#generate-new-key)
    - [Get public key](#get-public-key)
    - [Copy public key to server](#copy-public-key-to-server)
    - [Connect to server](#connect-to-server)
    - [`known_hosts` file](#known_hosts-file)
    - [Use multiple keys](#use-multiple-keys)
  - [On server](#on-server)
    - [Disable password authentication](#disable-password-authentication)
    - [Change SSH port](#change-ssh-port)
  - [Links](#links)

The **Secure Shell** (**SSH**) is a cryptographic network protocol for operating network services securely over an unsecured network.

## On client

### List existing keys

List existing SSH keys:

```bash
ls -al ~/.ssh
```

### Generate new key

Generate a new key pair:

```bash
cd ~/.ssh
ssh-keygen
# ssh-keygen -f gitlab
# ssh-keygen -t ed25519
```

### Get public key

Copy public key into clipboard:

```bash
cat ~/.ssh/id_rsa.pub | pbcopy
```

### Copy public key to server

```bash
ssh-copy-id -i ~/.ssh/gitlab remote_username@remote_host
```

This logs into the server host, and copies keys to the server, and configures them to grant access by adding them to the `authorized_keys` file. The copying may ask for a password or other authentication for the server.

Only the public key is copied to the server. The private key should never be copied to another machine.

After that connect to server and make sure that your public key was added there:

```bash
cat ~/.ssh/authorized_keys 
```

### Connect to server

If you have multiple privates keys, specify the one to use:

```bash
ssh -i path_to_private_key_file remote_username@remote_host
#ssh -i path_to_private_key_file remote_username@remote_hos -p SSH_PORT
```

This command assumes that your username on the remote system is the same as your username on your local system:

```bash
ssh remote_host
```

If your username is different on the remote system, you can specify it by using this syntax:

```bash
ssh remote_username@remote_host
```

To exit back into your local session, simply type:

```bash
exit
```

You can create an alias to connect quicker:

```bash
echo "alias mysite='ssh mywebsite.abcd'" >> ~/.zshrc
```

### `known_hosts` file

Path to `known_hosts` file:

```bash
vim ~/.ssh/known_hosts
```

### Use multiple keys

```bash
cd
touch .ssh/config
vim .ssh/config
```

Contents of the file:

```text
Host myshortname realname.example.com
    HostName realname.example.com
    IdentityFile ~/.ssh/realname_rsa
    User remoteusername

Host myother realname2.example.org
    HostName realname2.example.org
    IdentityFile ~/.ssh/realname2_rsa
    User remoteusername2
```

## On server

### Disable password authentication

On client:

```bash
ssh-copy-id aleksei@192.168.0.44
```

On server:

```bash
sudo vim /etc/ssh/sshd_config
```

Set:

```text
PasswordAuthentication no
```

Run:

```bash
sudo systemctl restart ssh
```

### Change SSH port

```bash
grep -i port /etc/ssh/sshd_config
```

If you want to change the default SSH port in Ubuntu, perform the following steps with root privileges:

Open the `/etc/ssh/sshd_config` file:

```bash
sudo vim /etc/ssh/sshd_config
```

and locate the line:

```text
#Port 22
```

Then, uncomment (Remove the leading`#` character) it and change the value with an appropriate port number (for example, 22000):

```text
Port 22000
```

Restart the SSH server:

```bash
systemctl restart sshd
```

When connecting to the server using the ssh command, you need to specify the port to connect using the `-p` flag:

```bash
ssh remote_username@remote_host -p SSH_PORT_NUMBER
```

## Links

- [↑ Best way to use multiple SSH private keys on one client](https://stackoverflow.com/questions/2419566/best-way-to-use-multiple-ssh-private-keys-on-one-client)
- [↑ Что записано в файле .ssh/known_hosts](https://habr.com/ru/post/421477/)
- [↑ How to Set Up SSH Keys on Ubuntu 20.04](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys-on-ubuntu-20-04)
