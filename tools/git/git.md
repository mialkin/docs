# Git

## Commands

| Command                                                     | Description                                                                    |
| ----------------------------------------------------------- | ------------------------------------------------------------------------------ |
| git branch -d BRANCH_NAME                                   | Delete local branch                                                            |
| git branch -m NEW_NAME                                      | Rename branch                                                                  |
| git push REMOTE_NAME -u BRANCH_NAME                         | Push local branch to remote                                                    |
| git push REMOTE_NAME --delete BRANCH_NAME                   | Delete remote branch                                                           |
| git remote add REMOTE_NAME https://github.com/user/repo.git | Add a remote                                                                   |
| git remote remove REMOTE_NAME                               | Remove remote                                                                  |
| git remote set-url REMOTE_NAME git://new.url.here           | Replace old remote URL with new one                                            |
| git remote -v                                               | Display list of remotes with origins                                           |
| git rm -fr --cached FOLDER_NAME                             | Removes caches of FOLDER_NAME folder. You can use dot (`.`) instead of folder. |
| vim .git/config                                             | Edit upstreams                                                                 |
