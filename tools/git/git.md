# Git

- [Git](#git)
  - [Config](#config)
  - [Commands](#commands)
  - [Push to multiple repositories](#push-to-multiple-repositories)
  - [.gitignore](#gitignore)
  - [Naming commits](#naming-commits)

## Config

Edit `.gitconfig` file:

```bash
vim ~/.gitconfig # or use: git config --global --edit
```

Set default branch name:

```bash
git config --global init.defaultBranch BRANCH_NAME
```

## Commands

| Command                                   | Description                                                                   |
| ----------------------------------------- | ----------------------------------------------------------------------------- |
| git branch -d BRANCH_NAME                 | Delete local branch                                                           |
| git branch -m NEW_NAME                    | Rename branch                                                                 |
| git log --oneline                         | Print only commits subject line (without body)                                |
| git push REMOTE_NAME -u BRANCH_NAME       | Push local branch to remote                                                   |
| git push REMOTE_NAME --delete BRANCH_NAME | Delete remote branch                                                          |
| git remote add REMOTE_NAME REMOTE_URL     | Add a remote                                                                  |
| git remote remove REMOTE_NAME             | Remove remote                                                                 |
| git remote set-url REMOTE_NAME REMOTE_URL | Replace old remote URL with new one                                           |
| git remote -v                             | Display list of remotes with origins                                          |
| git rm --cached FILENAME                  | Remove a file from cache                                                      |
| git rm -fr --cached FOLDER_NAME           | Removes caches of FOLDER_NAME folder. You can use dot (`.`) instead of folder |
| git shortlog                              | Summarizes git log so that each commit will be grouped by author and title    |
| vim .git/config                           | Edit upstreams                                                                |

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
- Use the body to explain *what* and *why* vs. *how*

[↑ How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit)
