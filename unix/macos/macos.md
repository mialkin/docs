# macOS

- [macOS](#macos)
  - [Applications](#applications)
    - [Finder](#finder)
    - [Fluor](#fluor)
    - [Gifox](#gifox)
    - [Keka](#keka)
    - [LosslessCut](#losslesscut)
    - [Lunar](#lunar)
    - [Mos](#mos)
    - [Rectangle](#rectangle)
    - [Rider](#rider)
      - [`rider` command](#rider-command)
    - [Safari](#safari)
    - [SensibleSideButtons](#sensiblesidebuttons)
    - [Sublime Text](#sublime-text)
    - [Visual Studio Code](#visual-studio-code)
  - [Terminal aliases](#terminal-aliases)
  - [TTL](#ttl)
  - [Flush DNS cache](#flush-dns-cache)
  - [Install developers' applications](#install-developers-applications)
  - [IP address](#ip-address)
  - [Key repeating](#key-repeating)
  - [Run application](#run-application)
  - [Copy text to clipboard](#copy-text-to-clipboard)
  - [User scripts folder](#user-scripts-folder)

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

### LosslessCut

[↑ LosslessCut](https://github.com/mifi/lossless-cut) is an application that cuts the data stream and directly copies it over.

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

#### `rider` command

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

### SensibleSideButtons

[↑ SensibleSideButtons](https://sensible-side-buttons.archagon.net).

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
alias dotwatch='dotnet watch --no-hot-reload'
```

## TTL

```bash
sysctl -w net.inet.ip.ttl           # Get current value
sudo sysctl -w net.inet.ip.ttl=65   # Set value to 65
```

## Flush DNS cache

```zsh
sudo killall -HUP mDNSResponder
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

## User scripts folder

```bash
ll /usr/local/bin
```

Check if folder is in `PATH` environment variable:

```bash
echo "$PATH" | tr ':' '\n'
```

Add path if it's not there:

```bash
echo 'export PATH="/usr/local/bin:$PATH"' >> ~/.zshrc
```
