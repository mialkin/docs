# macOS

- [macOS](#macos)
  - [Settings](#settings)
    - [Aliases](#aliases)
    - [Finder](#finder)
    - [System](#system)
      - [Desktop \& Dock](#desktop--dock)
      - [Focus](#focus)
      - [Trackpad](#trackpad)
  - [Applications](#applications)
    - [Brew](#brew)
    - [DBeaver Community](#dbeaver-community)
    - [Etcher](#etcher)
    - [ExifTool](#exiftool)
    - [FFmpeg](#ffmpeg)
    - [Fluor](#fluor)
    - [Gifox](#gifox)
    - [HandBrake](#handbrake)
    - [Keka](#keka)
    - [Lens](#lens)
    - [Logi Tune](#logi-tune)
    - [LosslessCut](#losslesscut)
    - [Lunar](#lunar)
    - [Mos](#mos)
    - [Nightfall](#nightfall)
    - [OBS](#obs)
    - [Rectangle](#rectangle)
    - [Rider](#rider)
      - [`rider` command](#rider-command)
    - [SensibleSideButtons](#sensiblesidebuttons)
    - [Sublime Text](#sublime-text)
    - [Visual Studio Code](#visual-studio-code)
  - [Miscellaneous](#miscellaneous)
    - [TTL](#ttl)
    - [Flush DNS cache](#flush-dns-cache)
    - [Install developers' applications](#install-developers-applications)
    - [IP address](#ip-address)
    - [Key repeating](#key-repeating)
    - [Run application](#run-application)
    - [Copy text to clipboard](#copy-text-to-clipboard)
    - [User scripts folder](#user-scripts-folder)

## Settings

### Aliases

```bash
echo "alias ll='ls -la'" >> ~/.zshrc && \
echo "alias cls='clear'" >> ~/.zshrc && \
echo "alias python='python3'" >> ~/.zshrc && \
echo "alias dotwatch='dotnet watch --no-hot-reload'" >> ~/.zshrc
```

Makefile [↑ autocompletion](https://stackoverflow.com/questions/33760647/makefile-autocompletion-on-mac):

```bash
echo "zstyle ':completion:*:*:make:*' tag-order 'targets'" >> ~/.zshrc && \
echo "autoload -U compinit && compinit" >> ~/.zshrc
```

### Finder

Always show hidden files in Finder:

```bash
defaults write com.apple.Finder AppleShowAllFiles true && \
killall Finder
```

Set `Downloads` as default folder:

<img src="macos-finder.png" width="400px" alt="Finder" />

Show path bar: View -> Show Path Bar.

### System

#### Desktop & Dock

Click wallpaper to reveal desktop -> Only in Stage Manager.

#### Focus

Uncheck Focus -> Share across devices.

#### Trackpad

More gestures -> Swipe between pages -> Off

## Applications

### Brew

[↑ Brew](https://brew.sh/) is the package manager for macOS and Linux.

### DBeaver Community

[↑ DBeaver Community](https://dbeaver.io/download) is a free cross-platform database tool.

### Etcher

[↑ Etcher](https://etcher.balena.io) is a cross-platform tool to flash OS images onto SD cards and USB drives safely and easily.

### ExifTool

[↑ ExifTool](https://exiftool.org) is a command-line application for reading, writing and editing meta information in a wide variety of files.

```bash
brew install exiftool
```

Output image info:

```bash
exiftool image.jpg
```

Update image tags:

```bash
export THE_DATE="2016:08:01 00:00:03";
exiftool -DateTimeDigitized=$THE_DATE -DateTimeOriginal=$THE_DATE image.jpg
```

Compare two images:

```bash
exiftool image1.jpg -diff image2.jpg --system:all -e
```

Only show files that do not have `DateTimeDigitized` set:

```bash
exiftool -filename -if 'not ($datetimeoriginal)' .
```

Only show files that do not have `DateTimeDigitized` set:

```bash
exiftool -filename -if 'not ($datetimedigitized)' .
```

Get all time information about the image:

```bash
exiftool -time:all image.jpg
```

Move all files that don't have `DateTimeOriginal` tag set on them from current folder to a subfolder:

```bash
exiftool -if 'not $datetimeoriginal' -Directory=mysubfolder .
```

### FFmpeg

[↑ FFmpeg](https://www.ffmpeg.org/) is the leading multimedia framework, able to decode, encode, transcode, mux, demux, stream, filter and play pretty much anything that humans and machines have created.

It supports the most obscure ancient formats up to the cutting edge.

[↑ macOS installation](https://trac.ffmpeg.org/wiki/CompilationGuide/macOS):

```bash
brew install ffmpeg
```

[↑ Concatenating](https://trac.ffmpeg.org/wiki/Concatenate) multiple files into one:

```bash
ffmpeg -f concat -i mylist.txt -c copy output.mp4
```

`mylist.txt`:

```text
file 'movie_1.mp4'
file 'movie_2.mp4'
file 'movie_3.mp4'
```

### Fluor

[↑ Fluor](https://github.com/Pyroh/Fluor) is a macOS application for switching Fn keys' mode based on active application.

```bash
brew install --cask fluor
```

### Gifox

[↑ Gifox](https://gifox.app) is a macOS status bar application for recording, converting, editing and sharing GIFs.

### HandBrake

[↑ HandBrake](https://handbrake.fr) is an application for converting video from nearly any format to a selection of modern, widely supported codecs.

### Keka

[↑ Keka](https://www.keka.io) is a macOS file archiver.

### Lens

[↑ Lens](https://k8slens.dev/) is a Kubernetes IDE.

### Logi Tune

[↑ Logi Tune](https://www.logitech.com/en-eu/video-collaboration/software/logi-tune-software.html) is a software for Logitech web cameras.

### LosslessCut

[↑ LosslessCut](https://github.com/mifi/lossless-cut) is an application that cuts the data stream and directly copies it over.

### Lunar

[↑ Lunar](https://github.com/alin23/Lunar) is a macOS application for controlling monitors.

```bash
brew install --cask lunar
```

### Mos

[↑ Mos](https://mos.caldis.me) is a lightweight tool used to smooth scrolling and set scroll direction independently for your mouse on macOS.

### Nightfall

[↑ Nightfall](https://github.com/r-thomson/Nightfall/) is an application that lets you manage macOS's dark mode from the menu bar.

### OBS

[↑ OBS Studio](https://obsproject.com/) is free and open source software for video recording and live streaming.

### Rectangle

[↑ Rectangle](https://rectangleapp.com) is an application that allows to move and resize windows in macOS using keyboard shortcuts or snap areas.

### Rider

#### `rider` command

Create file:

```bash
cd /usr/local/bin/ && \
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

### SensibleSideButtons

[↑ SensibleSideButtons](https://sensible-side-buttons.archagon.net) is an application that enables side navigation buttons on your third-party mice

May need to add this application to [↑Open at login](https://support.apple.com/en-is/guide/mac-help/mh15189/mac) after installation.

### Sublime Text

[↑ Sublime Text](https://www.sublimetext.com/) is a shareware text and source code editor available for Windows, macOS, and Linux.

```bash
echo 'export PATH="/Applications/Sublime Text.app/Contents/SharedSupport/bin:$PATH"' >> ~/.zprofile
```

```json
{
  "font_size": 13,
  "open_files_in_new_window": false
}
```

| Shortcut         | Description                   |
| ---------------- | ----------------------------- |
| <kbd>⌘ ⇧ L</kbd> | Selection -> Split into Lines |

### Visual Studio Code

[↑ Visual Studio Code](https://code.visualstudio.com)

[↑ Install `code` command in PATH](https://stackoverflow.com/a/68273710/1833895).

## Miscellaneous

### TTL

```bash
sysctl -w net.inet.ip.ttl           # Get current value
sudo sysctl -w net.inet.ip.ttl=65   # Set value to 65
```

### Flush DNS cache

```bash
sudo killall -HUP mDNSResponder
```

```bash
echo "alias flush-dns='sudo killall -HUP mDNSResponder'" >> ~/.zshrc
```

### Install developers' applications

1. Just move application into `/Applications` folder and open it
2. If you see `"YOUR_APPLICATION" cannot be opened because the developer cannot be verified`, please open up **System Preferences** → **Security & Privacy** → **General** → **Open Anyway**.
3. If you see the error `The application YOUR_APPLICATION can't be opened` error on launch, you could `chmod +x "/Applications/YOUR_APPLICATION.app/Contents/MacOS/YOUR_APPLICATION`"

### IP address

```bash
curl ifconfig.me
```

### Key repeating

Enable key repeating:

```bash
defaults write -g ApplePressAndHoldEnabled -bool false
```

Next, restart your computer and you should now be able to repeat all characters.

Replace `true` with `false` to revert changes back.

### Run application

```sh
open -a calculator                  # -a is for "application"
ll /Applications                    # List installed apps
open -a "Microsoft Remote Desktop"
```

### Copy text to clipboard

```sh
cat example.txt \| pbcopy
```

### User scripts folder

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
