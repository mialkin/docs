# macOS

- [macOS](#macos)
  - [Hotkeys](#hotkeys)
  - [Tree](#tree)
  - [Terminal](#terminal)
    - [Shortcuts](#shortcuts)
  - [IP address](#ip-address)
    - [Other](#other)
  - [Finder](#finder)
  - [Terminal aliases](#terminal-aliases)
  - [Key repeating](#key-repeating)
  - [Adding new path to PATH environment variable](#adding-new-path-to-path-environment-variable)
  - [Rider](#rider)
  - [Sublime](#sublime)
  - [Flush DNS cache](#flush-dns-cache)
  - [Hosts](#hosts)
  - [Installing app from developers](#installing-app-from-developers)

## Hotkeys

| Command                                                                                                          | Description                               |
| :--------------------------------------------------------------------------------------------------------------- | :---------------------------------------- |
| <kbd>⌘</kbd> + <kbd>⇧</kbd> + <kbd>G</kbd>                                                                       | Go to folder dialog in Terminal           |
| <kbd>⌘</kbd> + <kbd>⇧</kbd> + <kbd>.</kbd>                                                                       | Show hidden files and folders in Terminal |
| <kbd>⌘</kbd> + <kbd>C</kbd> on the file to copy, and <kbd>⌘</kbd> + <kbd>V</kbd> on the command line in Terminal | Get full path to a file in Terminal       |
| <kbd>⌥</kbd> + <kbd>⌘</kbd> + <kbd>H</kbd> + <kbd>M</kbd>                                                        | Minimize all windows                      |

## Tree

```bash
brew install tree
tree .
```

## Terminal

### Shortcuts

| Shortcut | Meaning                     |
| -------- | --------------------------- |
| Ctrl + F | Move forward one character  |
| Ctrl + B | Move backward one character |
| Ctrl + W | Remove word backward        |
| Ctrl + K | Delete to end of line       |
| Ctrl + Y | Paste                       |

## IP address

```bash
curl ifconfig.me
```

### Other

Run an application:

```sh
open -a calculator                  # -a is for "application"
ll /Applications                    # List installed apps
open -a "Microsoft Remote Desktop"
```

Copy text to clippbord:

```sh
cat example.txt \| pbcopy
```

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

## Rider

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

## Sublime

```bash
echo 'export PATH="/Applications/Sublime Text.app/Contents/SharedSupport/bin:$PATH"' >> ~/.zprofile
```

## Flush DNS cache

```zsh
sudo killall -HUP mDNSResponder
```

## Hosts

```zsh
vim /etc/hosts
```

## Installing app from developers

1. Just move app into `/Applications` folder and open it.
2. If you see `"YOUR_APP" cannot be opened because the developer cannot be verified.`, please open up System Preferences -> Security & Privacy -> General -> Open Anyway.
3. If you see the error `The application YOUR_APP can't be opened` error on launch, you could chmod +x "/Applications/YOUR_APP.app/Contents/MacOS/YOUR_APP"
