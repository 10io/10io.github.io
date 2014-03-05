---
layout: post
title: Git svn commands
tags: git
---

This is my personal cheat sheet for using [git](http://git-scm.com/) with svn servers. It contains all the main day-to-day commands I use. This is not a guide for git. Prior knowledge to git is required. So please [RTFM](http://git-scm.com/book/en/Git-and-Other-Systems-Git-and-Subversion).

## Config
In .git/config, add:

```bash
[core]
	whitespace = nowarn
	safecrlf = true
	diff = auto
	status = auto
	branch = auto
[user]
	email = foo@bar.net
	name = Foo Barish
[log]
	date = local
```

Caution: mind the tabs!

To avoid a mess with line endings:

```bash
$ git config --global core.autocrlf true
```

##Setup
Tracking a SVN repository with an exotic layout:

```bash
git svn clone svn://my/repo/project/foo -r$REV$:HEAD -T trunk -t tags/releases -b branches/releases FOO
```

$REV$ is a the svn revision number from where git will checkout. $REV$=0 can take a lot of time on large repositories.
-T indicates where the trunk is. Mandatory.
-t indicates where the tags are. Optional.
-b indicates where the branches are. Optional.

Add all svn ignores to git:

```bash
$ git svn show-ignore >> .git/info/exclude
```

##Usage
update

```bash
$ git svn rebase
```

hack hack hack

```bash
$ git add .
$ git commit -m "my foobar comment"
```

commit to svn

```bash
$ git dcommit
```

CAUTION: do not try to use git merge. It doesn't work (well, last time I tried, it didn't work at all :). [Period](http://stackoverflow.com/questions/2164690/using-git-svn-to-merge-a-svn-branch-back-into-trunk-and-trunk-back-into-the-branc). See below for an alternative.

## Tricks
### Create a local master branch from a remote one

```bash
$ git checkout -b master_remote_foo remotes/foo_branch
```

Then hack hack hack, then

```bash
$ git add . (<- caution with this)
$ git commit -m "foobarish"
$ git svn rebase
$ git svn dcommit
-> commiting to svn_repo/branches/foo_branch...
```

switch back to master trunk

```bash
$ git checkout master
```

### Tracking an exotic svn branch

```bash
$ git config --add svn-remote.newbranch.url https://svn/path_to_newbranch/
$ git config --add svn-remote.newbranch.fetch :refs/remotes/newbranch
$ git svn fetch newbranch [-r<rev>]
$ git checkout -b local-newbranch -t newbranch
$ git svn rebase newbranch
```
Work as normal: hack, hack, hack, git add, git commit, hack, git add, git commit, git svn dcommit. (thanks to this [answer](http://stackoverflow.com/questions/296975/how-do-i-tell-git-svn-about-a-remote-branch-created-after-i-fetched-the-repo))

Stop tracking a remote branch:

```bash
$ git branch -D -r branch_name #branch_name without 'remotes' prefix
$ rm -rf .git/svn/refs/remotes/branch_name
```

See [this](http://stackoverflow.com/questions/1839606/delete-a-svn-branch-via-git)

### Undo last commit
Undo last commit (the commit was done localy (git commit) but not remotely(no git svn dcommit done yet):

```bash
$ git reset --soft HEAD^
```

_hack hack hack_

```bash
$ git add
$ git commit -c ORIG_HEAD
```

Awesome trick thanks to [this](http://stackoverflow.com/questions/927358/git-undo-last-commit) or you can try (doesn't work for me)

```bash
$ git commit --amend
```

### Add files
Add files to staging area by reviewing them with diff

```bash
$ git add --patch
```

Add new, updated *and* removed files to staging area:

```bash
$ git add -A
```

### Stash
Stash usage:

```bash
$ git stash list
$ git stash save "wip my foobarish feature"
$ git stash pop [stash@{0}]
$ git stash drop [stash@{0}]
$ git stash -u # git stash save modified files + untracked files, available in git 1.7.7+
```

### Log
Viewing the log as tree

```bash
$ git log --graph --oneline --all --since="3 days ago"(<- awesome! :)
```

Alternative: gitk provides a little more information in one glance:

```bash
$ gitk --all
```

git log is provided with many filters (git help log is your very best friend).

### Merge commits
About merging a svn branch onto master, here is an awesome [trick](http://ariejan.net/2010/06/10/cherry-picking-specific-commits-from-another-branch). This method is very reliable, use it over cherry-picking a range of commits (cherry-pick will silently fail if a commit fails to merge... meh?). Cherry-picking is perfect to merge one single commit, not a range. You will need a merge tool. You can choose any merge tool you like. I use p4merge, simple to use, effective and rainbow colors! Set the mergetool in the main git config file. Git config example for [p4merge](http://www.perforce.com/product/components/perforce_visual_merge_and_diff_tools) on Windows:

```bash
[merge]
	tool = p4merge
[mergetool "p4merge"]
	path = C:/APP/Perforce/p4merge.exe
	keepTemporaries = false
	trustExitCode = false
	keepBackup = false
```

Ok, let's start, shall we? Let's say we have master tracking trunk and master_svn_branch tracking an svn branch. We want to merge revision A to B from the svn branch back to trunk

```bash
$ git checkout master_svn_branch B # checkout svn branch at revision B
$ git rebase -i --onto master A^
```

The last command is where the magic happens. We are telling git to rebase revision A to B onto branch master. Since, we passed option -i, git will present all the commits and give us the choice to merge each commit onto master (default), to squash several commits into one (<- awesome!) or even ignore some commits.

Now as git will rebase the commits, conflicts may appear, git will interrupt the rebase and wait for manual conflict resolution. Easy, we just

```bash
$ git mergetool
```

and git will feed the merge tool with each conflict. Once all conflicts are resolved, simple continue the rebase:

```bash
$ git rebase --continue
```

Note: when squashing commits, git will present the generated commit message to have chance to modify it.

If a problem occurs, or you want to abort the rebase

```bash
git rebase --abort
```

will cancel all changes. Now, once the rebase is complete, if you inspect the master_svn_branch (with gitk) you'll see that, we are tracking the master branch with all the merged commits.

Sometimes git rebase and/or your merge tool leave some backup files behind. Don't worry, use:

```bash
$ git clean -n
$ git clean -f
```

Now it is time to check the merge, run some tests. Once everything is alright, simple git svn dcommit and you're done! The commits will go to master, which is tracking remote/trunk!
CAUTION: branch master_svn_branch is not tracking remote/svn_branch anymore. It's better to delete it to avoid messy situations.

Rebasing commits is far more reliable than an svn merge. Best part: commits done by Jenkins (release commits) can be ignored from the merge.

This is the ultimate reason why git > svn.

### Revert a commit
revert a specific commit by creating the reverse commit:

```bash
$ git revert -n SHA1_COMMIT
```

this will create and stage the reverse changes for commit SHA1_COMMIT. Yep, all of this in one command.

### Fatal error:bad object
In case of some weird mess with the index like "fatal error: bad object" or "index mismatch", you can try this brute force cleaning:

```bash
rm -rf ./.git/svn
git svn fetch
This will reconstruct the local svn commits database. You will see a message like this one

Migrating from a git-svn v1 layout...
Data from a previous version of git-svn exists, but
    .git/svn
    (required for this version (1.7.10) of git-svn) does not exist.
Done migrating from a git-svn v1 layout
```

This reconstruction will take a (hell of a) lot of time.

### Goodies and Refs
* [Git](http://git-scm.com/)
* [The Git Book](http://progit.org/book)
* [Gource](http://code.google.com/p/gource/)
