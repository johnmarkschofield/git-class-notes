# Git Training

## Instructor

> Peter Bell (Trainer (contract) at GitHub)
> Peter is a contract member of the GitHub training team, writing a book on Git for Pearson, one on GitHub for O’Reilly and creating screencasts on GitHub for Code School and Pluralsight. He’s also a co-founder and co-organizer of CTO school and the New York Startup CTO summit, runs a node.js meetup and is the founder and CTO of speakgeek.co - a company that helps business people to learn how to more effectively hire and manage developers. http://linkedin.com/in/peterfbell

pbell@github.com

[@peterbell](http://twitter.com/peterbell)




## Git Class I

### Class Introduction
From http://chefconf2014.busyconf.com/schedule#activity_52f94ed06c7f049afd000005
> Learn how to leverage the power of git and github to better manage your code and/or your infrastructure configuration. In this fast paced, hands on class you'll learn the basics of git and how to use branching to keep your projects under control. We'll then look at how to share your code on github and collaborate with others most effective via cloning, collaborating, forking and pull requests. If you'd like to go beyond the basics, consider also taking the intermediate and advanced class in the afternoon.



### Configuring Git

```
git config --global user.name
# Displays your username

git config --global user.name "Foobar Tweedly"
# Sets username

git config --global user.email
# Displays email

git config --global user.email foorbar@tweedly.com
# Sets email
```

For auto-line endings, use core.autocrlf:

```
# On Mac and Linux
$ git config --global core.autocrlf input

# on windows
git config --global core.autocrlf true

# Everywhere:
git config --global color.ui true
```

There are three different levels of configuration in git: local, global, system.

Most of the time you use global. Applies to everything at the user level.

System level is for all users of the system.

Local level is for local repo.


#### Decentralized Version Control

Don't need permission to create a repository. Create one on your own system.

```
git status
```


Everything is stored in .git directory in root of repo.

```
git status -s
# Gives you a shorter summary. Very cool.

$ git config --global alias.s "status -s"
# Now "git s" is an alias for "git status -s"
```

So cool.

```
git commit -m "Added home page"
[master (root-commit) 567e74e] Added home page
 2 files changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 index.css
 create mode 100644 index.html

 ```
 Root-commit is something you see only on first commit to a repo.
 Hash is a unique ID for that commit.


### Git architecture

Working directory, staging area (aka index), history

Staging area analagous to shopping cart at amazon.com

"git add" adds to staging area
"git commit" sends staging area to history

Red is in working directory but nowhere else.
Green is in staging area.



#### History
```
git log
git log --online
git log --oneline --decorate # Gives information about head and branches
$ git config --global alias.lg "log --oneline --decorate --graph --all"
```

Aliases (and all global configs) are stored in ~/.gitconfig



#### Moving files.

If you do "git mv filea fileb" then it knows about the move.

If you do "mv filea fileb" without git then git is confused and thinks the first file is deleted and the 2nd file is created.

```
# git status
# On branch master
# Changes not staged for commit:
#   (use "git add/rm <file>..." to update what will be committed)
#   (use "git checkout -- <file>..." to discard changes in working directory)
#
#   deleted:    index.css
#
# Untracked files:
#   (use "git add <file>..." to include in what will be committed)
#
#   home.css
no changes added to commit (use "git add" and/or "git commit -a")
$ git s
 D index.css
?? home.css

$ git add -A
```
Then it will add the delete and auto-detect the rename. It detects a rename if the files are > 50% identical.

This means that if you have two files that are > 50% the same, it could be detected as a rename when it's not. Be careful.

```
git rm filename
# removes the file

rm fileb
# git sees an unstaged deletion

git add . # does not work

git add -A # adds deletion

touch test.log

git status

# Git knows about test.log, but we don't want to commit it. You could remember to never add it. But you'll screw it up.
echo "*.log" >> .gitignore
git status # Now git is happy

$ git config --global core.excludesfile ~/.gitignore
# Probably don't use this

```


```
git remote add origin FOOBAR
```

Origin is always the main repo. Could be anything, but origin is convention.

```
git push -u origin master # the -u sets the origin as upstream for master
```

https://help.github.com/articles/set-up-git -- do this and cache credentials on OS X



```
$ git push
warning: push.default is unset; its implicit value is changing in
Git 2.0 from 'matching' to 'simple'. To squelch this message
and maintain the current behavior after the default changes, use:

  git config --global push.default matching

To squelch this message and adopt the new behavior now, use:

  git config --global push.default simple

See 'git help config' and search for 'push.default' for further information.
(the 'simple' mode was introduced in Git 1.7.11. Use the similar mode
'current' instead of 'simple' if you sometimes use older versions of Git)

Everything up-to-date
```

Do the one that ends in simple.

matching pushes all branches that have matching branches on remote.

simple pushes only changes to current branch to remote.


#### Branching

```
git branch # list branches
git branch about_us # create about_us branch
git branch
git checkout about_us
touch about.html
touch about.css
git add about.html
git commit -m "We've got some smokin' html here."
git add about.css
git commit -m "Our CSS is da bomb"
git status
$ git lg
* fef1e04 (HEAD, about_us) Our CSS is da bomb
* 60899b4 We've got some smokin' html here.
* 8564a0a (origin/master, master) New home page
```

HEAD is the stuff currently in the working directory

To create a branch and check it out:

```
git checkout -b contact_us # does git branch and then git checkout
```

Now we want to merge about_us branch into master branch.

We should work on feature branch. Most feature branches should be short-lived -- an hour, a day, etc.

If you have a feature branch living for more than a week, it's probably too large.

Rule of thumb is master is always releasable. Even if you're not doing CD and deploying 20 times a day

once feature branch is done we merge back into master branch.

QA should test branch before we merge into master.


So we check out master and then merge into it.

--no-ff = don't do fast-forward. Use this; don't do fast-forward

```
git merge about_us # forgetting to --no-ff
git reset --keep master@{1} # Undo previous merge
git merge --no-ff about_us # Complete the merge

# Now delete the branch:
git branch -d about_us

# This does not remove history or commits
```


Good idea to add no-ff by default:

```
$ git config --global --add merge.ff false
```

```
git show HASH # shows you details
```



All work should be in branches


As soon as you push to github you can't change that history.




## Git Class II

From http://chefconf2014.busyconf.com/schedule#activity_52f94eb26c7f049afd000003

> Learn how to fix almost anything with amend, reset, revert, rebase and even the magical reflog. Learn about alternative workflows and best practices for collaboration using github and git. Whether you're comfortable with the basics of adding and committing to git, have a little experience with branching and know how to clone a github repo or have a couple of years of solid git experience, by the end of this class you'll understand git more deeply and be able to use it more elegantly. If you're completely new to git, consider taking the foundations class in the morning first so you're ready for this afternoon session.


### .git directory

.git/info/exclude -- a local exclude list that you should not use. Not shared with anyone else.

.git/objects/*
Directories named with the first two characters of the SHA hash.

After committing one file once (only one commit) there are three objects that git stores:
* The commit itself
* tree * number of directories changed
* blob * number of files changed

Look at the contents of one of those objects:
```
$ git cat-file -p 95d0 # include first two characters of directories
```




### Rebasing

Changing the base (starting point) of a branch

So for example, rebasing content_us page so that it happens after merging branch about_us

Gives you a nice, clean, linear history.
Allows you to keep master releasable. Since you're testing against the rebased branch, you're testing something very similar to what master will be after you merge.

Three types of tags:
* lightweight
* annotated
* signed

Git defaults to annotated tags

Signed tags: cryptographically sign tags

```
git tag -a 0.1
```

Tag everything that goes into production.

Let's do a hotfix on tag 0.1

```
git checkout 0.1
git checkout -b rb0.1
do fix
git commit -m "fix"
git tag -a 0.1.1
git checkout master
git merge --no-ff rb0.1
```

You need the ability to have your taggesd master OR a release branch released to production.

```
git reset --hard HEAD~1 # throws away last commit


git checkout feature2
git rebase master
```


### Collaboration

git pull -- does a git fetch, then git merge orgin master into my master


When you get merge error on pushing or pulling because you have local commits

```
git rebase origin/master
git push origin master
```

To avoid that:

```
git fetch
git merge origin/master
```

or if you have local commits:
```
git rebase master
```


A better way:
```
git pull --rebase # does the rebase
```

Solving that problem -- feature branches

```
git checkout -b branchname
# make changes
git push -u origin branchname
```

Who should merge a pull request? Some say that the person who created it can NOT merge it. Certainly someone else should review it, but Peter thinks that whoever created the PR should merge it after it's approved.

Whoever creates a pull request should be the one merging it in.

```
git branch -d branchname
git push origin :branchname # deletes branch on server.
```

for feature branches that can't be done in a week, implement feature flags and don't show new partial feature i production. Do this to reduce amount of integration.


### Undoing almost anything!

```
$ git lg
* 7671557 (HEAD, master) this is a bad idea
* 7465dee First psot

$ git revert 7671557
[master 5f72c2c] Revert "this is a bad idea"
 1 file changed, 0 insertions(+), 0 deletions(-)
 delete mode 100644 bad_idea.html
```


revert is safe to do with shared commits (if pushed to github)
It's kind of ugly. But it's safe and preserves history.


Do the following only if the changes have not been pushed to the server.

```
$ git reset --hard HEAD~2 # throw away last 2 commits
```


Edit a commit message:
```
$ git commit --amend # ONLY for the very last commit
```

Add a file to staging, then git commit --amend will add missing file.

```
git reset # throw away one or more commits
```

```
git reset HEAD~2 # throw away last two commits
```


Types of reset:
```
git reset (hard, soft, or mixed) deletes:
                          history   Staging   Working Directory
   --soft:                    x
   --mixed (default):         x        x
   --hard:                    x        x             x
```


Merging two commits into one:
```
git reset --soft HEAD~2
```


###Going to the reflog

```
git reflog # good for 30 days or so
git reset --hard HASH
```

or 

```
git checkout HASH
```



```
git rebase -i
```
Merge multiple commits into one; change order of them, etc.
