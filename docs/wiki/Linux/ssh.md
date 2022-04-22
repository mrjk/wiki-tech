# SSH


## SSH with git

To load a custom key:
```
GIT_SSH_COMMAND='ssh -v -i ssh_keys/deploy-scripts@bitbucket_rsa4096_20210727 -o IdentitiesOnly=yes'  git clone git@gitlab.example:7999/project/deploy-scripts.git
```

