---
title: "Trimming old Commits from Github"
#date: 2019-08-14T15:30:00-06:00
last_modified_at: 2019-10-02T10:30:00-07:00
categories:
  - Git
tags:
  - GitHub
  - How-To
---

Im sure there is a much more efficient way of doing this, but I had to remove about 2000 commits from my repo.. this is how I achieved it.

* TOC
{:toc}

## Step 1: Identify the commit to be your new root

For this I simply looked in Github, at the commit history. And I selected this one (my first commit)

![Gihub Commit](../../assets/images/2019-08-17-Trimming-old-Commits-from-Github/1.png)

My commit was ```295c4c6ece076396eef2d8477d768d4c255b14b9```

## Step 2: Clone and Trim all commits before

Check out a new branch with that commit hash
```powershell
git checkout -b oldroot 295c4c6ece076396eef2d8477d768d4c255b14b9
git write-tree 
```
Note the output. Mine was ```00fb96df77166974bacb5bfa16e28cbd1baf8fbd```
Take that new, add a commit message and commit-tree
```powershell
git commit-tree -m "New History Rebase" 00fb96df77166974bacb5bfa16e28cbd1baf8fbd
```
Note the output again. Mine was ```64e19740fb1b78d89c3ca3bf6cf60d124caf7c14```
Checkout a new branch with this new root, and rebase into master.

```powershell
git checkout -b newroot 64e19740fb1b78d89c3ca3bf6cf60d124caf7c14
git rebase --onto newroot oldroot master
# repeat for other branches than master that should use the new initial commit
git checkout master
git branch -D oldroot
git branch -D newroot
```
## Step 3: Actully trim

**Danger Notice:** Make sure you have a backup!!!
{: .notice--danger}

Execute this:
```powershell
git gc 
```

## Step 4: Clean up the remote (github)

Im sure there is a much better way to do this.... but this is what worked for me.
```powershell
git checkout newmaster
git push --set-upstream origin newmaster
```
Now we need to go to github and change the default branch away from master (to new master). If you dont do this, the remote delete will be rejected.
![Gihub Commit](../../assets/images/2019-08-17-Trimming-old-Commits-from-Github/2.png)

Once done...we delete the remote master (and local master); then create a new master branch.

```powershell
git push origin --delete master
git branch -D master
git checkout -b master
git push --set-upstream origin master
```
Now go back to where you were on github before; and change the default back to master again; then we remove the newmaster branch.

```powershell
git push origin --delete newmaster
git branch -D newmaster
```

And done... all old commits before your hash should be removed, and the branches cleaned up.

## Step 5: *New Addition* - Remove Tags

You also need to remove tags as well.. Best to use linux (or WSL) for this.

Remove tags from local repo:
```bash
git tag -d `git tag | grep -E '.'`
```

Remove tags from github:
```bash
git ls-remote --tags origin | awk '/^(.*)(\s+)(.*[a-zA-Z0-9])$/ {print ":" $2}' | xargs git push origin
```