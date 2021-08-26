# Command - Git

## Initialize
In the project's folder run:
```bash
git init 
```

## Repository status
```bash
git status 
```

## Logs
Show commits of current branch
```bash
git log
```
Squeeze info in one line
```bash
git log --oneline
```
Show which files are affected and how many lines of them
```bash
git log --stat
```
Show which files are affected and how
```bash
git log --patch
```
Squeeze info in one line
```bash
git log --oneline
```
Show info about a spesific commit
```bash
git show [options] <sha>
```
Show all branches and how are connected
```bash
git log --oneline --graph --all
```

## Fetching
`Fetch` branches and/or tags (collectively, "refs") from one or more other repositories, along with the objects necessary to complete their histories. Remote-tracking branches are updated.

Before fetching, `remove` any remote-tracking references that no longer exist on the remote. Tags are not subject to pruning if they are fetched only because of the default tag auto-following or due to a --tags option.
```bash
git fetch --all --prune
```

## Branching

### Show local branches only
```bash
git branch
```
or
```bash
git branch -l
```

### Show remote branches only
```bash
git branch -r
```

### Show all local and remote branches
```bash
git branch -a 
```

### Create a new branch
Create a new branch but stay in the current one:
```bash
git branch <branch-name>
```
Create and switch to the new branch:
```bash
git checkout -b <branch-name>
```
or
```bash
git branch <branch-name>
git checkout <branch-name>
```
Create a branch from a specific commit:
```bash
git branch <branch-name> <sha>
```

### Merge two branches
Let's say we want to merge `feature-101` into `develop`.
After we have committed all our changes in `feature-101`
```bash
git checkout develop
git merge feature-101
```

### Rename a local branch
If you are on the branch you want to rename:
```bash
git branch -m <new-branch-name>
```
If you are on a different branch:
```bash
git branch -m <old-branch-name> <new-branch-name>
```

### Delete a local branch
In order to delete a _merged_ branch we run:
```bash
git branch -d <branch-name>
```
If the branch we intend to remove is not merged yet, we need to force the action:
```bash
git branch -D <branch-name>
```

### Delete a remote branch
```bash
git push <remote-name> --delete <branch-name>
```

### Prune tracking branches not on the remote
```bash
git remote prune origin
```

## Tagging

Tag the latest commit in the branch
```bash
git tag -a "tag name"
```

Tag a past commit
```bash
git tag -a "tag name" <sha>
```

Remove tag
```bash
git tag -d "tag name"
```

## Modifying the last commit

### Changing The Last Commit
If your Working Directory is clean (meaning there aren't any uncommitted changes in the repository), then running `git commit --amend` will let you provide a new commit message. Your code editor will open up and display the original commit message. Just fix a misspelling or completely reword it! Then save it and close the editor to lock in the new commit message.

### Add Forgotten Files To Commit
Alternatively, `git commit --amend` will let you include files (or changes to files) you might've forgotten to include. Let's say you've updated the color of all navigation links across your entire website. You committed that change and thought you were done. But then you discovered that a special nav link buried deep on a page doesn't have the new color. You could just make a new commit that updates the color for that one link, but that would have two back-to-back commits that do practically the exact same thing (change link colors).

Instead, you can amend the last commit (the one that updated the color of all of the other links) to include this forgotten one. To do get the forgotten link included, just:
```text
    edit the file(s)
    save the file(s)
    stage the file(s)
    and run git commit --amend
```
So you'd make changes to the necessary CSS and/or HTML files to get the forgotten link styled correctly, then you'd save all of the files that were modified, then you'd use git add to stage all of the modified files (just as if you were going to make a new commit!), but then you'd run git commit --amend to update the most-recent commit instead of creating a new one.

[Source](https://classroom.udacity.com/courses/ud123/lessons/f02167ad-3ba7-40e0-a157-e5320a5b0dc8/concepts/e176503b-3eae-4b22-a1b3-2953bab3d5e5)

---

## Tips & Tricks

### Merge develop to master
```bash
(on branch development)$ git merge master
git checkout master
git merge --no-ff develop
```
[Source](https://stackoverflow.com/a/14168817)

## Appendix A - Sources:
- [gitready - list remote branches](http://gitready.com/intermediate/2009/02/13/list-remote-branches.html)
- [Git Branching - Basic Branching and Merging](https://git-scm.com/book/en/v2/Git-Branching-Basic-Branching-and-Merging)
- [Git Branching - Branch Management](https://git-scm.com/book/en/v2/Git-Branching-Branch-Management)
- [Koukia - Delete a local and a remote GIT branch](https://koukia.ca/delete-a-local-and-a-remote-git-branch-61df0b10d323)
- [Git Documentation - git-fetch](https://git-scm.com/docs/git-fetch)
