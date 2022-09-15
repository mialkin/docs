# User managment

## Creating

Create a new user `sammy`:

```bash
sudo adduser sammy --disabled-password
```

The `--disabled-password` option will not set a password, meaning no password is legal, but login is still possible (for example with SSH RSA keys).

## Switching

To switch to a user `sammy` that was created without password, run:

```bash
sudo -u sammy -s
```

To exit run:

```bash
exit
```

## Listing

To find out if a user with name `sammy` exists run:

```bash
getent passwd | grep sammy
```

Local user information is stored in the `/etc/passwd` file. Each line in this file represents login information for one user:

```bash
less /etc/passwd
```

Each line in the file has seven fields delimited by colons that contain the following information:

- User name.
- Encrypted password (`x` means that the password is stored in the `/etc/shadow` file).
- User ID number (UID).
- User's group ID number (GID).
- Full name of the user (GECOS).
- User home directory.
- Login shell (defaults to `/bin/bash`).

If you want to find out how many users accounts you have on your system, pipe the `getent passwd` output to the `wc` command:

## Deleting

Delete user with its data including `/home/sammy` folder:

```bash
sudo userdel -r sammy
```

## Elevate permissions

To elevate yourself to super use run:

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

## Allowing to connect via SSH

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

If you're using the root account to set up keys for a user account, it's also important that the ~/.ssh directory belongs to the user and not to root:

```bash
chown -R sammy:sammy ~/.ssh
```

Test connection by running:

```bash
ssh sammy@yoursite.xyz
```
