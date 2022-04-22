# Git


## Tips and tricks

Delete a branch locally, then remotely:
```
git branch -d mybranch
git push origin --delete mybranch
```

Cleanup unexinsting branches:
```
git remote prune origin --dry-run
git remote prune origin
```

Find heavy commits in a git repo ([source](https://stackoverflow.com/questions/10622179/how-to-find-identify-large-commits-in-git-history)):
```
git rev-list --objects --all |
  git cat-file --batch-check='%(objecttype) %(objectname) %(objectsize) %(rest)' |
  sed -n 's/^blob //p' |
  sort --numeric-sort --key=2 |
  cut -c 1-12,41- |
  $(command -v gnumfmt || echo numfmt) --field=2 --to=iec-i --suffix=B --padding=7 --round=nearest
```
