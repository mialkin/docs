# macOS

## Aliases

Set up aliases:

```sh
echo "alias ll='ls -la'\nalias cls='clear'" >> ~/.zshrc
```

## Key repeating

Enable key repeating:

```bash
defaults write -g ApplePressAndHoldEnabled -bool false
```

Next, restart your computer and you should now be able to repeat all characters.

## Adding new path to PATH environment variable

Open `.zshrc` file:

```bash
vim ~/.zshrc
```

Add line at the end of the file:

```text
export PATH="/Users/aleksei/Library/Python/3.8/bin:${PATH}"
```

Save file and source it:

```bash
source .zshrc
```

## Hosts

```bash
vim /etc/hosts
```
