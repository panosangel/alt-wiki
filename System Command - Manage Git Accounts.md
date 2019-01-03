# System Commands - Manage Git Accounts

## Set global configuration
```bash
nano ~/.gitconfig
```

## Set config per repository
In the repository's folder run:
```bash
nano .git/config
```

## Common settings
Select the preferable editor, in my case this is `nano`.
```bash
git config --global core.editor "nano"
```

## Single account
In case we have only one git account then we can set the following options globally:
```bash
git config --global user.name "<username>"
git config --global user.email "<email>"
```

## Multiple accounts
In case we multiple accounts we can set the following options per repository.
In the repository's folder run:
```bash
git config user.name "<username>"
git config user.email "<email>"
```

### macOS
```bash
git config --global credential.helper "osxkeychain"
git config --global credential.useHttpPath "true"
```

## Appendix A - Sources
- [Using multiple accounts with git or Github](https://coderwall.com/p/9ub-6a/using-multiple-accounts-with-git-or-github)
- [How do I make Git use the editor of my choice for commits?](https://stackoverflow.com/questions/2596805/how-do-i-make-git-use-the-editor-of-my-choice-for-commits)
- [How do I push to GitHub under a different username?](https://stackoverflow.com/questions/13103083/how-do-i-push-to-github-under-a-different-username)
