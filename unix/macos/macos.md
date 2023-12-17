# macOS

- [macOS](#macos)
  - [Edit `PATH` environment variable](#edit-path-environment-variable)
  - [Flush DNS cache](#flush-dns-cache)
  - [`hosts` file](#hosts-file)
  - [Install developers' applications](#install-developers-applications)
  - [IP address](#ip-address)
  - [Key repeating](#key-repeating)
  - [Run application](#run-application)
  - [Copy text to clipboard](#copy-text-to-clipboard)
  - [Applications](#applications)
    - [Finder](#finder)
    - [Fluor](#fluor)
    - [Gifox](#gifox)
    - [Keka](#keka)
    - [Lunar](#lunar)
    - [Mos](#mos)
    - [Rectangle](#rectangle)
    - [Rider](#rider)
    - [Safari](#safari)
    - [Sublime Text](#sublime-text)
    - [Visual Studio Code](#visual-studio-code)
  - [Terminal aliases](#terminal-aliases)
  - [TTL](#ttl)

## Edit `PATH` environment variable

Open `.zshrc` file:

```zsh
vim ~/.zshrc
```

Insert a new line at the end of the file:

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

## `hosts` file

```zsh
sudo vim /etc/hosts
```

## Install developers' applications

1. Just move application into `/Applications` folder and open it
2. If you see `"YOUR_APPLICATION" cannot be opened because the developer cannot be verified`, please open up **System Preferences** → **Security & Privacy** → **General** → **Open Anyway**.
3. If you see the error `The application YOUR_APPLICATION can't be opened` error on launch, you could `chmod +x "/Applications/YOUR_APPLICATION.app/Contents/MacOS/YOUR_APPLICATION`"

## IP address

```bash
curl ifconfig.me
```

## Key repeating

Enable key repeating:

```zsh
defaults write -g ApplePressAndHoldEnabled -bool false
```

Next, restart your computer and you should now be able to repeat all characters.

Replace `true` with `false` to revert changes back.

## Run application

```sh
open -a calculator                  # -a is for "application"
ll /Applications                    # List installed apps
open -a "Microsoft Remote Desktop"
```

## Copy text to clipboard

```sh
cat example.txt \| pbcopy
```

## Applications

### Finder

Always show hidden files in Finder:

```zsh
defaults write com.apple.Finder AppleShowAllFiles true
killall Finder
```

Set `Downloads` as default folder:

<img src="macos-finder.png" width="400px">

### Fluor

[↑ Fluor](https://github.com/Pyroh/Fluor) is a macOS application for switching Fn keys' mode based on active application.

```bash
brew install --cask fluor
```

### Gifox

[↑ Gifox](https://gifox.app) is a macOS status bar application for recording, converting, editing and sharing GIFs.

### Keka

[↑ Keka](https://www.keka.io) is a macOS file archiver.

### Lunar

[↑ Lunar](https://github.com/alin23/Lunar) is a macOS application for controlling monitors.

```bash
brew install --cask lunar
```

### Mos

[↑ Mos](https://mos.caldis.me) is a lightweight tool used to smooth scrolling and set scroll direction independently for your mouse on macOS.

### Rectangle

[↑ Rectangle](https://rectangleapp.com) is an application that allows to move and resize windows in macOS using keyboard shortcuts or snap areas.

### Rider

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

### Safari

Clear local storage: **Web inspector** → **Storage** → **Clear local storage**

### Sublime Text

```bash
echo 'export PATH="/Applications/Sublime Text.app/Contents/SharedSupport/bin:$PATH"' >> ~/.zprofile
```

### Visual Studio Code

[↑ Install `code` command in PATH](https://stackoverflow.com/questions/29955500/code-is-not-working-in-on-the-command-line-for-visual-studio-code-on-os-x-ma).

## Terminal aliases

Set up aliases in `~/.zshrc` file:

```zsh
alias ll='ls -la'
alias cls='clear'
alias python='python3'
```

## TTL

```bash
sysctl -w net.inet.ip.ttl           # Get current value
sudo sysctl -w net.inet.ip.ttl=65   # Set value to 65
```
