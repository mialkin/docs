# User managment

## Creating

Create a new user `bob`:

```bash
sudo adduser bob --disabled-password
```

The `--disabled-password` option will not set a password, meaning no password is legal, but login is still possible (for example with SSH RSA keys).

## Listing

To find out if a user with name `bob` exists run:

```bash
getent passwd | grep bob
```

Local user information is stored in the `/etc/passwd` file. Each line in this file represents login information for one user:

```bash
less /etc/passwd
```

Each line in the file has seven fields delimited by colons that contain the following information:

* User name.
* Encrypted password (`x` means that the password is stored in the `/etc/shadow` file).
* User ID number (UID).
* User’s group ID number (GID).
* Full name of the user (GECOS).
* User home directory.
* Login shell (defaults to `/bin/bash`).

If you want to find out how many users accounts you have on your system, pipe the `getent passwd` output to the `wc` command:

## Deleting

Delete user with its data including `/home/bob` folder:

```bash
sudo userdel -r bob
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
