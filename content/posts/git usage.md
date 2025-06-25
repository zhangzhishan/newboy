---
title: git 基本操作
tag: git
date: 2019-03-26 09:36:55
---
## git config
git 的自动换行会有些问题，有的时候会出现`fatal error`，故可以配置为自动转化为`lf`，或者关闭这个功能。

```
git config --global core.autocrlf input #提交到git是自动将换行符转换为lf
git config --global core.editor "vim" # use vim editor
```

## git add files to last commit

git commit --amend --no-edit

git push -f origin some_branch

git clean -f

git clean -d

git prune

git cherry-pick

`git merge --no-ff`

git add -p            # patch mode可以将一个文件分成多份来进行提交，适合于当一次内容里面修改了多方面的东西的时候。


## git commit
### commit messages
第一行为summary，少于50个字符，然后空一行，下面是正文，正文一行也要控制在72个字符。
### reset head to some commit

## setup difftool and merge tool
```
git config --global difftool.beyondcompare.cmd "\"c:\program files\beyond compare 4\bcomp.exe\" \"$LOCAL\" \"$REMOTE\""
git config --global diff.tool beyondcompare
```

```
git config --global merge.tool p4merge
git config --global mergetool.p4merge.path "C:/Program Files/Perforce/p4merge.exe"
```

## delete  a branch

```
git push -d origin branchname
git branch -d branchname
```

## checkout using remote branch
首先`git fetch` 获取到所有的branch,然后执行
`git checkout -t <name of remote>/test` or `git checkout -b test <name of remote>/test`

## rebase local branch with remote branch
git pull --rebase origin master

## git stash
```
git stash save XXX
git stash list
git stash apply
```

## git rebase
rebase有两个方面，一个是个人部分，需要利用`git rebase -i`对已有的commit进行整理，另一方面是对于branch层面上的，避免merge的时候出现分叉，使用`git pull --rebase`来进行merge，同时也可以有两个设置

```
git config branch.dev.rebase true
git config --global branch.autosetuprebase always
```

## merge 的几种操作

* rebase
* squash merge

![rebase](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1577241461_20181121151120571_16166.png)

![squash](https://raw.githubusercontent.com/zhangzhishan/blogpics/dev/1577241463_20181121151151708_5552.png)

## conflicts

the meaning of `theirs` and `ours` are not the same in `git merge` and `git rebase`, [reference.](https://stackoverflow.com/questions/2959443/why-is-the-meaning-of-ours-and-theirs-reversed-with-git-svn/2960751#2960751)
```
git checkout --theirs -- .
git checkout --ours .  # checkout our local version of all files
git add -u             # mark all conflicted files as merged
```

## push some commit
Cherry-pick works best compared to all other methods while pushing a specific commit.
The way to do that is:

Create a new branch:

```
git branch <new-branch>
```
Update your new-branch with your origin branch:

```
git fetch
git rebase
```
These actions will make sure that you exactly have the same stuff as your origin has.
Cherry-pick the sha id that you want to do push:

```
git cherry-pick <sha id of the commit>
```
Push it to your origin:

```
git push
```
Run `gitk` to see that everything looks the same way you wanted.
## break a previous commit to multiple commit
`git rebase -i` will do it.

First, start with a clean working directory: git status should show no pending modifications, deletions, or additions.

To split apart your most recent commit, first:

`$ git reset HEAD~`

Now commit the pieces individually in the usual way, producing as many commits as you need.

If it was farther back in the tree, then

`$ git rebase -i HEAD~3`

where 3 is how many commits back it is.

If it was farther back in the tree than you want to count, then

`$ git rebase -i 123abcd~`

where `123abcd` is the SHA1 of the commit you want to split up.

When you get the rebase edit screen, find the commit you want to break apart. At the beginning of that line, replace `pick` with `edit` (`e` for short). Save the buffer and exit. Rebase will now stop just after the commit you want to edit. Then:

`$ git reset HEAD~`

Commit the pieces individually in the usual way, producing as many commits as you need, then

`$ git rebase --continue`

## some tips
做 `git push` 之前记得做一次 `git pull --rebase origin master`.
## git log
### show name recent changed
git log --name-status -10 path/to/dir
## git revert
`git revert commit-id`,  add a new commit to revert that commit

`git checkout <commit_hash> -- <file>`

## git clean untracked file
`git clean -fd`

## git reflog
`git reflog` to get records before index.

## change default editor to visual studio code
`git config --global core.editor "code --wait"`
> [呼呼呼山](http://code4fun.me)
> 2019-03-26 09:36:55