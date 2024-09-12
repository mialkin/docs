# GNU Privacy Guard, GPG

**GNU Privacy Guard** or **GnuPG** or **GPG** is a command line tool with features for easy integration with other applications.

GPG settings and keys, if exist, reside in the `~/.gnupg` folder:

## Table of contents

- [GNU Privacy Guard, GPG](#gnu-privacy-guard-gpg)
  - [Table of contents](#table-of-contents)
  - [Installation](#installation)
  - [Generate keys](#generate-keys)
  - [List and delete keys](#list-and-delete-keys)
  - [Signing Git commits](#signing-git-commits)
  - [Pinentry](#pinentry)

## Installation

Install [↑ GnuPG](https://gnupg.org), a complete and free implementation of the OpenPGP standard as defined by [↑ RFC4880](https://www.ietf.org/rfc/rfc4880.txt):

```bash
brew install gnupg
```

## Generate keys

```bash
gpg --full-generate-key
```

## List and delete keys

```bash
gpg --list-keys
gpg --delete-key YOUR_USER_ID
gpg --delete-secret-key YOUR_USER_ID
```

## Signing Git commits

```bash
# Get the public key using key ID, example:
gpg --armor --export 12345678902799C8C2913642203DD9F35CC69AFD

# Set the key ID in git:
git config --global user.signingkey 12345678902799C8C2913642203DD9F35CC69AFD

# Sign all commits by default in any local repository
git config --global commit.gpgsign true
```

The second command from above adds `signingKey` line to `~/.gitconfig` file:

```text
[user]
	email = john@doe.xyz
	name = John Doe
	signingkey = 12345678902799C8C2913642203DD9F35CC69AFD
```

Git is cryptographically secure, but it's not foolproof. If you're taking work from others on the internet and want to verify that commits are actually from a trusted source. You can sign commits and tags locally, to give other people confidence about the origin of a change you have made.

For most individual users, GPG or SSH will be the best choice for signing commits. S/MIME signatures are usually required in the context of a larger organization. SSH signatures are the simplest to generate.

Generating a GPG signing key is more involved than generating an SSH key, but GPG has features that SSH does not. A GPG key can expire or be revoked when no longer used. GitHub shows commits that were signed with such a key as "Verified" unless the key was marked as compromised. SSH keys don't have this capability.

```bash
git commit -S -m "YOUR_COMMIT_MESSAGE" # Sign single commit
git config commit.gpgsign true # Sign commits by default for a local repository
```

## Pinentry

Install [↑ Pinentry](https://www.gnupg.org/related_software/pinentry/index.html), a small collection of dialog programs that allow GnuPG to read passphrases and PIN numbers in a secure manner:

```bash
brew install pinentry-mac
```

Make sure the Pinentry shows a GUI prompt by running:

```bash
echo GETPIN | pinentry-mac
```
