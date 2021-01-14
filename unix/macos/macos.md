# macOS

## Always show hidden files in Terminal

```zsh
defaults write com.apple.Finder AppleShowAllFiles true
killall Finder
```

Replace `true` with `false` to revert changes back.

## Aliases

Set up aliases:

```zsh
echo "alias ll='ls -la'\nalias cls='clear'" >> ~/.zshrc
```

## Key repeating

Enable key repeating:

```zsh
defaults write -g ApplePressAndHoldEnabled -bool false
```

Next, restart your computer and you should now be able to repeat all characters.

## Adding new path to PATH environment variable

Open `.zshrc` file:

```zsh
vim ~/.zshrc
```

Add line at the end of the file:

```text
export PATH="/Users/aleksei/Library/Python/3.8/bin:${PATH}"
```

Save file and source it:

```zsh
source .zshrc
```

## Hosts

```zsh
vim /etc/hosts
```
