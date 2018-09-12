# GIT

## Table of Contents
- [Log viewing](#log_viewing)
- [Tags](#tags)
- [Repo Cleanup](#repo_cleanup)
- [Remove a branch](#remove_branch)
- [Remote tracking](#remote_tracking)
- [Repo config](#repo_config)
- [Stash](#stash)
- [Setup SSH keys](#ssh_keys)

<a name="log_viewing"></a>
## Log viewing
- Simple view
`git log --graph --decorate --abbrev-commit --pretty=oneline`
![git_simple_log](resources/git_simple_log.png)

- Log with date/time + author
`git log --graph --decorate --abbrev-commit --pretty=format:"%C(cyan)%h %C(yellow)%ad%Cred%d %Cgreen [%an:%ae] %Creset%s" --decorate --date=iso`
![git_date_log](resources/git_date_log.png)

<a name="tags"></a>
## Tags
- `git tag -l | sort -V` - See tags in the sort order
- `git push --tags` - Push tags to remote

<a name="repo_cleanup"></a>
## Repo Cleanup
```
git reflog expire --expire=now --all
git gc --prune=now
git stash clear
```

<a name="remove_branch"></a>
## Remove a branch
- Remove branch locally
```
git branch -d {the_local_branch}
```

- Remove remote branch
```
git push origin --delete {the_remote_branch}
```

<a name="remote_tracking"></a>
## Remote tracking
- To view remote tracking of the branches
-- simple command
```
git branch -vv
```
-- more detailed view
```
git remote show origin
```

- List all currently configured remotes
```
git remote -v
```

- To set remote of a repository
```
git remote add <remote_name> <url>
```

- To set remote of a branch
```
git branch branch_name --set-upstream-to <remote_name/branch_name>
```

- To remove upstream tracking of a branch
```
git branch --unset-upstream <branch_name>
```

- Show information about a remote
```
git remote show <remote>
```

- Download all changes from remote but don't merged into HEAD
```
git fetch <remote>
```

- Download changes and merge/integrated into HEAD
```
git pull <remote> <branch>
```

- Push local commits to remote
```
git push <remote> <branch>
```

<a name="repo_config"></a>
## Repo config
- View user and email used for git commit
```
git config user.name
git config user.email
```

- To set user + email
```
git config user.name "<user-name>"
git config user.email "<user-email-address>"
```

<a name="stash"></a>
## Stash
- Temporarily saves changes of half-done work

- `git stash` - Temporarily stores all modified tracked files
- `git stash pop` - Restores the most recently stashed files
- `git stash list` - Lists all stashed change sets
- `$ git stash drop` - Discards the most recently stashed change sets

<a name="ssh_keys"></a>
## Setup SSH keys
- [Setup SSH keys](https://help.github.com/articles/connecting-to-github-with-ssh/)
