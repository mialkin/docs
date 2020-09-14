# macOS

Set up aliases:

```sh
echo "alias ll='ls -la'\nalias cls='clear'" >> ~/.zshrc
```

Enable key repeating:

```bash
defaults write -g ApplePressAndHoldEnabled -bool false
```

Next, restart your computer and you should now be able to repeat all characters.
