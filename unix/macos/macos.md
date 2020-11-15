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

## PATH environment variable

Open `.zshrc` file:

```bash
vim ~/.zshrc
```

Add anywhere in the file the following line:

```text
export PATH="/Users/aleksei/Library/Python/3.8/bin:${PATH}"
```

Source file:

```bash
source .zshrc
```
