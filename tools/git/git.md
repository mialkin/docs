# Git

- [Git](#git)
  - [Git config](#git-config)
    - [Work vs personal credentials](#work-vs-personal-credentials)
  - [Commands](#commands)
  - [Push to multiple repositories](#push-to-multiple-repositories)
  - [.gitignore](#gitignore)
  - [Rebase](#rebase)
  - [Naming commits](#naming-commits)
  - [Fetching vs pulling](#fetching-vs-pulling)
    - [Fetching](#fetching)
    - [Pulling](#pulling)
  - [Stash](#stash)
    - [Pop](#pop)
    - [Apply](#apply)
    - [Drop](#drop)
    - [Show](#show)

## Git config

Edit global `.gitconfig` file:

```bash
vim ~/.gitconfig # or use: git config --global --edit
```

Set default branch name and always pull with rebase:

```bash
git config --global init.defaultBranch BRANCH_NAME
git config --global pull.rebase true
```

To see where that setting is defined (global, user, repo, etc...):

```bash
git config --list --show-origin
```

### Work vs personal credentials

`.gitconfig` file:

```text
[user]
	email = personal@gmail.com
	name = John Doe
[init]
	defaultBranch = main
[includeIf "gitdir:~/work/"]
    path = .gitconfig-work
```

`.gitconfig-work`:

```text
[user]
	email = work@company.com
	name = John Work
```

Check username and password inside _specific_ repository:

```bash
git config user.name
git config user.email
```

## Commands

| Command                                       | Description                                                                   |
| --------------------------------------------- | ----------------------------------------------------------------------------- |
| git branch -d BRANCH_NAME                     | Delete local branch                                                           |
| git branch -m NEW_NAME                        | Rename branch                                                                 |
| git branch -r                                 | List all remote branches                                                      |
| git checkout -b NEW_BRANCH                    | Create new branch from current                                                |
| git cherry-pick COMMIT_HASH                   | Cherry pick commit                                                            |
| git config --list                             | Show all config sections                                                      |
| git config --list --show-origin               | Show where settings are defined (global, user, repo, etc...)                  |
| git config SETTING_NAME                       | Check what current settings are (in this case username)                       |
| git config user.name                          | Check current value of `username` setting                                     |
| git config user.name "John Doe"               | Set local user name                                                           |
| git config user.email your@email.com          | Set local user email                                                          |
| git config --global user.email your@email.com | Set global user email                                                         |
| git log --oneline                             | Print only commits subject line (without body)                                |
| git mv README README.md                       | Rename file                                                                   |
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

## Rebase

```bash
git checkout -b my-feature-branch-backup
git checkout my-feature-branch
git fetch origin main
git rebase origin/main
```

## Naming commits

-   Separate subject from body with a blank line
-   Limit the subject line to 50 characters
-   Capitalize the subject line
-   Do not end the subject line with a period
-   Use the imperative mood in the subject line
-   Wrap the body at 72 characters
-   Use the body to explain _what_ and _why_ vs. _how_

[↑ How to Write a Git Commit Message.pdf](How%20to%20Write%20a%20Git%20Commit%20Message.pdf)

[↑ How to Write a Git Commit Message — Original Article](https://chris.beams.io/posts/git-commit)

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

## Stash

```bash
git stash
```

At this point you're free to make changes, create new commits, switch branches, and perform any other Git operations; then come back and re-apply your stash when you're ready.

Note that the stash is local to your Git repository; stashes are not transferred to the server when you push.

### Pop

You can reapply previously stashed changes with:

```bash
git stash pop
git stash pop stash@{n}
```

Popping your stash removes the changes from your stash and reapplies them to your working copy.

### Apply

Alternatively, you can reapply the changes to your working copy and keep them in your stash with:

```bash
git stash apply
git stash apply stash@{n}
```

This is useful if you want to apply the same stashed changes to multiple branches.

Now that you know the basics of stashing, there is one caveat with git stash you need to be aware of: by default Git _won't_ stash changes made to untracked or ignored files.

### Drop

```bash
git stash drop            # drop top hash, stash@{0}
git stash drop stash@{n}  # drop specific stash - see git stash list
```

### Show

```bash
git stash show -p
git stash show -p stash@{n}  ## show specific stash
```

[↑ Git stash](https://www.atlassian.com/git/tutorials/saving-changes/git-stash)
