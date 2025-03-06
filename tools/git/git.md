# Git

- [Git](#git)
  - [Git config](#git-config)
    - [Work vs personal credentials](#work-vs-personal-credentials)
  - [Commands](#commands)
  - [Push to multiple repositories](#push-to-multiple-repositories)
  - [List remote branches and the last commit's author](#list-remote-branches-and-the-last-commits-author)
  - [.gitignore](#gitignore)
  - [Rebase](#rebase)
  - [Merge](#merge)
  - [Naming commits](#naming-commits)
  - [Fetching vs pulling](#fetching-vs-pulling)
    - [Fetching](#fetching)
    - [Pulling](#pulling)
  - [Change commit date](#change-commit-date)
    - [macOS date](#macos-date)
  - [Update forked repository from original repository](#update-forked-repository-from-original-repository)
  - [`git log` format](#git-log-format)
  - [Git hooks](#git-hooks)
  - [Custom scripts](#custom-scripts)
    - [`git-set-config`](#git-set-config)
    - [`git-create-repos`](#git-create-repos)

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

`.gitconfig-work` (lies inside `~` folder next to `.gitconfig`):

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

| Command                                           | Description                                                                     |
| ------------------------------------------------- | ------------------------------------------------------------------------------- |
| git branch -d BRANCH_NAME                         | Delete local branch                                                             |
| git branch -m NEW_NAME                            | Rename branch                                                                   |
| git branch -r                                     | List all remote branches                                                        |
| git branch -a -v                                  | List all branches verbose                                                       |
| git checkout -b NEW_BRANCH                        | Create new branch from current and check it out                                 |
| git checkout BRANCH_NAME                          | Switch to existing branch                                                       |
| git cherry-pick COMMIT_HASH                       | Cherry pick commit                                                              |
| git cherry-pick -n COMMIT_HASH                    | Cherry pick without commit                                                      |
| git commit --amend -m "An updated commit message" | Modify the most recent commit                                                   |
| git config --list                                 | Show all config sections                                                        |
| git config --list --show-origin                   | Show where settings are defined (global, user, repo, etc...)                    |
| git config SETTING_NAME                           | Check what current settings are (in this case username)                         |
| git config user.name                              | Check current value of `username` setting                                       |
| git config user.name "John Doe"                   | Set local user name                                                             |
| git config user.email your@email.com              | Set local user email                                                            |
| git config --global user.email your@email.com     | Set global user email                                                           |
| git log --oneline                                 | Print only commits subject line (without body)                                  |
| git mv README README.md                           | Rename/move file preserving history                                             |
| git mv folder folder2                             | Rename/move folder preserving history                                           |
| git push REMOTE_NAME -u BRANCH_NAME               | Push local branch to remote                                                     |
| git push REMOTE_NAME --delete BRANCH_NAME         | Delete remote branch                                                            |
| git push REMOTE_NAME TAG_NAME                     | Push certain tag to remote                                                      |
| git push REMOTE_NAME --tags                       | Push all tags to the remote server that are not already there                   |
| git remote add REMOTE_NAME REMOTE_URL             | Add a remote                                                                    |
| git remote remove REMOTE_NAME                     | Remove remote                                                                   |
| git remote set-url REMOTE_NAME REMOTE_URL         | Replace old remote URL with new one                                             |
| git remote -v                                     | Display list of remotes with origins                                            |
| git rm --cached FILENAME                          | Remove a file from cache                                                        |
| git rm -fr --cached FOLDER_NAME                   | Removes caches of FOLDER_NAME folder. You can use dot (`.`) instead of folder   |
| git shortlog                                      | Summarizes git log so that each commit will be grouped by author and title      |
| git stash apply                                   | Apply changes and keep them in stash                                            |
| git stash apply stash@{n}                         | Apply changes and keep them in stash                                            |
| git stash drop                                    | Drop top hash — `stash@{0}`                                                     |
| git stash drop stash@{n}                          | Drop specific stash — see `git stash list`                                      |
| git stash list                                    | List stashes                                                                    |
| git stash pop                                     | Apply changes and remove them from stash                                        |
| git stash pop stash@{n}                           | Apply changes and remove them from stash                                        |
| git stash push -m "My stash name"                 | Stash changes with message                                                      |
| git stash show                                    |                                                                                 |
| git stash show -p stash@{n}                       | Show specific stash                                                             |
| git tag                                           | List tags                                                                       |
| git tag TAG_NAME                                  | Create a [↑ lightweight](https://git-scm.com/book/en/v2/Git-Basics-Tagging) tag |
| git tag -a TAG_NAME -m 'Switch to Postgres'       | Create an annotated tag with message                                            |
| git tag -a TAG_NAME COMMIT_HASH                   | Create a new tag for certain commit                                             |
| git show TAG_NAME                                 |                                                                                 |
| git tag -d TAG_NAME                               | Delete tag                                                                      |

## Push to multiple repositories

Set push remotes:

```bash
git remote set-url --add --push origin git@gitlab.com:mialkin/YOUR_REPOSITORY_NAME.git
git remote set-url --add --push origin git@github.com:mialkin/YOUR_REPOSITORY_NAME.git
```

Print all remotes' fetch/push URLs:

```bash
git remote -v
```

Set fetch remote:

```bash
git remote set-url origin git@gitlab.com:mialkin/YOUR_REPOSITORY_NAME.git
```

Example of resulting config:

```text
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = git@github.com:mialkin/YOUR_REPOSITORY_NAME.git
	fetch = +refs/heads/*:refs/remotes/origin/*
	pushurl = git@github.com:mialkin/YOUR_REPOSITORY_NAME.git
	pushurl = git@gitlab.com:mialkin/YOUR_REPOSITORY_NAME.git
[branch "main"]
	remote = origin
	merge = refs/heads/main
```

## List remote branches and the last commit's author

List remote branches and the last commit's author and author date for each branch.

Sort by most recent commit's author date.

```bash
# https://gist.github.com/l15n/3103708
for branch in `git branch -r | grep -v HEAD`;do echo -e `git show --format="%ai %ar by %an" $branch | head -n 1` \\t$branch; done | sort -r
```

Add `| grep "John Doe"` at the end, to filter output by specific author name.

## .gitignore

```gitignore
# Ignore directory
directory-to-ignore/
```

## Rebase

```bash
git checkout -b my-feature-branch-backup
git checkout my-feature-branch
git fetch origin main
git rebase origin/main
```

Cancel rebase:

```bash
git rebase --abort
```

## Merge

Cancel merge:

```bash
git merge --abort
```

## Naming commits

- Separate subject from body with a blank line
- Limit the subject line to 50 characters
- Capitalize the subject line
- Do not end the subject line with a period
- Use the imperative mood in the subject line
- Wrap the body at 72 characters
- Use the body to explain _what_ and _why_ vs _how_

PDF: [How to Write a Git Commit Message](how-to-write-a-git-commit-message.pdf).

[↑ How to Write a Git Commit Message](https://chris.beams.io/posts/git-commit) — original article.

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

## Change commit date

Simple solution:

```bash
git commit --date="10 day ago" -m "Your commit message"
```

Warning: `git --date` will only modify the `GIT_AUTHOR_DATE`, so depending on the circumstances you'll see the current date attached to the commit `GIT_COMMITTER_DATE`.

To make a commit that looks like it was done in the past you have to set both `GIT_AUTHOR_DATE` and `GIT_COMMITTER_DATE` environment variables:

```bash
GIT_AUTHOR_DATE=$(date -d "...") GIT_COMMITTER_DATE="$GIT_AUTHOR_DATE" git commit -m "..."
```

Above `date -d "..."` can be exact date like `2019-01-01 12:40:04` or relative like `24 days ago`.

The author date on a commit is preserved on rebase/cherry-pick etc, but the committer date is changed.

As the "Pro Git Book" explains: "The author is the person who originally wrote the work, whereas the committer is the person who last applied the work".

In the context of dates, the `GIT_AUTHOR_DATE` is the date the file was changed, whereas the `GIT_COMMITTER_DATE` is the date it was committed. It is important to note that by default, `git log` displays author dates as "Date" but then uses commit dates for filtering when given a `--since` option.

### macOS date

```bash
brew install coreutils
gdate -d "28 days ago"
```

or just:

```bash
date -v-28d
```

## Update forked repository from original repository

```bash
git remote add upstream ssh://example.com/some-project/original-repository.git
git remote -v
git fetch upstream master
git checkout feature/ABC-123-branch
git rebase upstream/master
```

[↑ How to Update Fork Repo From Original Repo](https://levelup.gitconnected.com/how-to-update-fork-repo-from-original-repo-b853387dd471).

## `git log` format

Open `.gitconfig` file:

```bash
vim ~/.gitconfig
```

Add the following lines:

```text
[format]
	pretty = format:%C(yellow)%h %C(cyan)%ad %C(cyan)%d %Creset%s %C(red)%aN
```

[↑ The shortest possible output from git log containing author and date](https://stackoverflow.com/questions/1441010/the-shortest-possible-output-from-git-log-containing-author-and-date).

## Git hooks

A **Git hook** is a script that runs automatically every time a particular event occurs in a Git repository.

Git hook scripts are useful for identifying simple issues before submission to code review. By running on every commit hooks automatically point out issues in code such as missing semicolons, trailing whitespace, and debug statements. By pointing these issues out before code review, this allows a code reviewer to focus on the architecture of a change while not wasting time with trivial style nitpicks.

Hooks can reside in either local or server-side repositories, and they are only executed in response to actions in that repository.

Hooks reside in the `.git/hooks` directory of every Git repository. Git automatically populates this directory with example scripts when you initialize a repository.

To "install" a hook, all you have to do is remove the `.sample` extension. Hooks need to be executable, so you may need to change the file permissions of the script if you're creating it from scratch.

[↑ Git hooks](https://www.atlassian.com/git/tutorials/git-hooks).

## Custom scripts

See [User scripts folder](/unix/macos/macos.md#user-scripts-folder).

### `git-set-config`

Create file:

```bash
cd /usr/local/bin && \
touch git-set-config && \
chmod +x git-set-config
```

Paste content:

```bash
#!/bin/bash

if [ $# -eq 0 ]
  then
    echo "Supply repository name"
    exit 1
fi

USERNAME=mialkin
REPOSITORY_NAME=$1

cat <<EOF > ./.git/config
[core]
	repositoryformatversion = 0
	filemode = true
	bare = false
	logallrefupdates = true
	ignorecase = true
	precomposeunicode = true
[remote "origin"]
	url = git@github.com:${USERNAME}/${REPOSITORY_NAME}.git
	fetch = +refs/heads/*:refs/remotes/origin/*
	pushurl = git@github.com:${USERNAME}/${REPOSITORY_NAME}.git
	pushurl = git@gitlab.com:${USERNAME}/${REPOSITORY_NAME}.git
	pushurl = git@gitflic.ru:${USERNAME}/${REPOSITORY_NAME}.git
[branch "main"]
	remote = origin
	merge = refs/heads/main
EOF
```

### `git-create-repos`

Create file:

```bash
cd /usr/local/bin && \
touch git-create-repos && \
chmod +x git-create-repos
```

Paste content:

```bash
#!/bin/bash

if [ $# -eq 0 ]
  then
    echo "Supply repository name"
    exit 1
fi

REPOSITORY_NAME=$1

############################### GitFlic ###############################
curl \
--request POST \
--header "Authorization: token GITFLIC_ACCESS_TOKEN" \
--header "Content-Type: application/json" \
--data "{\"title\": \"${REPOSITORY_NAME}\", \"isPrivate\": \"true\"}" \
--url "https://api.gitflic.ru/project"

############################### GitHub ###############################
curl \
--location \
--request POST \
--header "Accept: application/vnd.github+json" \
--header "Authorization: Bearer GITHUB_ACCESS_TOKEN" \
--header "X-GitHub-Api-Version: 2022-11-28" \
--data "{\"name\":\"${REPOSITORY_NAME}\",\"private\":false\"}" \
--url "https://api.github.com/user/repos"


############################### GitLab ###############################
curl \
--request POST \
--header "PRIVATE-TOKEN: GITLAB_ACCESS_TOKEN" \
--header "Content-Type: application/json" \
--data "{\"name\": \"${REPOSITORY_NAME}\"}" \
--url "https://gitlab.com/api/v4/projects/"
```
