# Git

- [Git](#git)
  - [Config](#config)
  - [Commands](#commands)
  - [Push to multiple repositories](#push-to-multiple-repositories)
  - [.gitignore](#gitignore)
  - [Naming commits](#naming-commits)
  - [Fetching vs pulling](#fetching-vs-pulling)
    - [Fetching](#fetching)
    - [Pulling](#pulling)

## Config

Edit global `.gitconfig` file:

```bash
vim ~/.gitconfig # or use: git config --global --edit
```

Set default branch name:

```bash
git config --global init.defaultBranch BRANCH_NAME
```

To see where that setting is defined (global, user, repo, etc...):

```bash
git config --list --show-origin
```

## Commands

| Command                                       | Description                                                                   |
| --------------------------------------------- | ----------------------------------------------------------------------------- |
| git branch -d BRANCH_NAME                     | Delete local branch                                                           |
| git branch -m NEW_NAME                        | Rename branch                                                                 |
| git config --list                             | Show all config sections                                                      |
| git config --list --show-origin               | Show where settings are defined (global, user, repo, etc...)                  |
| git config SETTING_NAME                       | Check what current settings are (in this case username)                       |
| git config user.name                          | Check current value of `username` setting                                     |
| git config user.name "John Doe"               | Set local user name                                                           |
| git config user.email your@email.com          | Set local user email                                                          |
| git config --global user.email your@email.com | Set global user email                                                         |
| git log --oneline                             | Print only commits subject line (without body)                                |
| git push REMOTE_NAME -u BRANCH_NAME           | Push local branch to remote                                                   |
| git push REMOTE_NAME --delete BRANCH_NAME     | Delete remote branch                                                          |
| git remote add REMOTE_NAME REMOTE_URL         | Add a remote                                                                  |
| git remote remove REMOTE_NAME                 | Remove remote                                                                 |
| git remote set-url REMOTE_NAME REMOTE_URL     | Replace old remote URL with new one                                           |
| git remote -v                                 | Display list of remotes with origins                                          |
| git rm --cached FILENAME                      | Remove a file from cache                                                      |
| git rm -fr --cached FOLDER_NAME               | Removes caches of FOLDER_NAME folder. You can use dot (`.`) instead of folder |
| git shortlog                                  | Summarizes git log so that each commit will be grouped by author and title    |
| vim .git/config                               | Edit upstreams                                                                |

## Push to multiple repositories

```bash
git remote set-url --add --push origin https://gitlab.com/mialkin/YOUR_REPOSITORY_NAME.git
git remote set-url --add --push origin https://github.com/mialkin/YOUR_REPOSITORY_NAME.git
```

## .gitignore

| Line           | Description      |
| -------------- | ---------------- |
| dir_to_ignore/ | Ignore directory |

## Naming commits

- Separate subject from body with a blank line
- Limit the subject line to 50 characters
- Capitalize the subject line
- Do not end the subject line with a period
- Use the imperative mood in the subject line
- Wrap the body at 72 characters
- Use the body to explain _what_ and _why_ vs. _how_

[↑ How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit)

## Fetching vs pulling

`git fetch` and `git pull` both are used to download new data from a remote repository.

### Fetching

`git fetch` really only downloads new data from a remote repository - but it doesn't integrate any of this new data into your working files:

```bash
git fetch origin
```

Fetch is great for getting a fresh view on all the things that happened in a remote repository.
Due to it's "harmless" nature, you can rest assured: fetch will never manipulate, destroy, or screw up anything. This means you can never fetch often enough.

### Pulling

`git pull`, in contrast, is used with a different goal in mind: to update your current HEAD branch with the latest changes from the remote server:

```bash
git pull origin master
```

This means that pull not only downloads new data; it also directly integrates it into your current working copy files.

Since "git pull" tries to merge remote changes with your local ones, a so-called "merge conflict" can occur. It's highly recommended to start a "git pull" only with a clean working copy. This means that you should not have any uncommitted local changes before you pull. Use Git's stash feature to save your local changes temporarily.
