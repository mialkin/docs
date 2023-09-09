# User management

## Table of contents

- [User management](#user-management)
  - [Table of contents](#table-of-contents)
  - [Create](#create)
  - [Switch](#switch)
  - [List](#list)
  - [Delete](#delete)
  - [Elevate permissions](#elevate-permissions)
  - [Groups](#groups)
  - [Add password to user](#add-password-to-user)
  - [Add to `sudo` group](#add-to-sudo-group)
  - [Allow to connect via SSH](#allow-to-connect-via-ssh)

## Create

Create a new user `sammy`:

```bash
sudo adduser sammy --disabled-password
```

The `--disabled-password` option will not set a password, meaning no password is legal, but login is still possible, for example with SSH RSA keys.

## Switch

Switch to a user `sammy` that was created without password:

```bash
sudo -u sammy -s
```

Exit:

```bash
exit
```

## List

Find out if a user with name `sammy` exists:

```bash
getent passwd | grep sammy
```

Local user information is stored in the `/etc/passwd` file. Each line in this file represents login information for one user:

```bash
less /etc/passwd
```

Each line in the file has seven fields delimited by colons that contain the following information:

- Username
- Encrypted password; `x` means that the password is stored in the `/etc/shadow` file
- User ID number, UID
- User's group ID number, GID
- Full name of the user, GECOS
- User home directory
- Login shell, defaults to `/bin/bash`

## Delete

Delete user with its data including `/home/sammy` folder:

```bash
sudo userdel -r sammy
```

## Elevate permissions

Elevate yourself to superuser:

```bash
sudo -i
```

Execute command and then run `exit` to log out to become an ordinary user:

```bash
cd /root
exit
```

## Groups

Show all existing groups:

```bash
cat /etc/group
```

Show group to which current user belongs:

```bash
groups
```

## Add password to user

```bash
sudo passwd sammy
```

## Add to `sudo` group

```bash
usermod -aG sudo sammy
```

## Allow to connect via SSH

Being logged in as a new user create new `.ssh` directory:

```bash
mkdir ~/.ssh
```

Copy your public key to `authorized_keys` file:

```bash
echo public_key_string >> ~/.ssh/authorized_keys
```

Recursively remove all "group" and "other" permissions for the ~/.ssh/ directory:

```bash
chmod -R go= ~/.ssh
```

If you're using the root account to set up keys for a user account, it's also important that the `~/.ssh` directory belongs to the user and not to root:

```bash
chown -R sammy:sammy ~/.ssh
```

Test connection:

```bash
ssh sammy@domain.xyz
```
