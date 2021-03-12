# macOS

## Finder

Always show hidden files in Finder:

```zsh
defaults write com.apple.Finder AppleShowAllFiles true
killall Finder
```

Set `Downloads` as default folder:

<img src="macosFinder.png" width="400px">

Replace `true` with `false` to revert changes back.

## Terminal aliases

Set up aliases in `~/.zshrc` file:

```zsh
alias ll='ls -la'
alias cls='clear'
alias python='python3'
alias k='kubectl'
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

## Flush DNS cache

```zsh
sudo killall -HUP mDNSResponder
```

## Launch Rider from terminal

Create file:

```bash
cd /usr/local/bin/
vim rider
```

With content:

```text
#!/bin/sh

open -na "Rider.app" --args "$@"
```

Change access mode:

```bash
chmod 775 rider
```

## Hosts

```zsh
vim /etc/hosts
```
