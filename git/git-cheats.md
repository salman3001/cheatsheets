### create new branch

```
git checkout -b new/branchname
```

### switch branch

```
git checkout  other/branchname
```

### update origin tracking branches

```
git fetch
```

```
git fetch orgin
```

```
git fetch upstream
```

### Merge remote tracking branch in active branch

```
git checkout current/brach
git merge remote/tracking/branch
```

### Create new branch from remote tracking branch

```
git checkout -b new/branchname remote/branch
```

### pull remote changes without creating extra merge commit

This keeps your local commits asside and apply remote changes directly and re apply local changes on top

```
git pull --rebase origin <branch-name>
```

this updates the remote tracking branches first and then rebase the local brach. keeps the local commits asside and reply the remote commits on top

```
git fetch origin
git rebase origin/<branch-name>
```

if no local changes, then merge remote tracking branch without merge commit

```
git merge --ff-only origin/<branch-name>
```

### delete local branch

or -D to force

```
git branch -d branch/name
```

### delete remote branch

or -D for force

```
git push -d origin remote/tracking/branch
```

### remove references to deleted remote branches.

```
git remote prune origin
```

or

```
git fetch --prune

```

### stash changes tracked changes

```
git stash message
```

### stash changes all changes

```
git stash -a message
```

### check stash list

```
git stash list
```

### apply stash

```
git stash apply stash-name
```
