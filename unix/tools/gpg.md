# GNU Privacy Guard (GPG)

GnuPG, also known as GPG, is a command line tool with features for easy integration with other applications

```bash
brew install gnupg
gpg --full-generate-key
gpg --list-secret-keys --keyid-format=long
gpg --armor --export 3AA5C34371567BD2 # Prints the GPG key ID, in ASCII armor format
```

## Signing Git commits

```bash
git commit -S -m "YOUR_COMMIT_MESSAGE" # Sign single commit
git config commit.gpgsign true # Sign commits by default for a local repository
git config --global commit.gpgsign true # Sign all commits by default in any local repository
```

Further reading:

- [↑ Generating a new GPG key](https://docs.github.com/en/authentication/managing-commit-signature-verification/generating-a-new-gpg-key)
- [↑ Signing commits with GPG](https://docs.gitlab.com/ee/user/project/repository/gpg_signed_commits/)
