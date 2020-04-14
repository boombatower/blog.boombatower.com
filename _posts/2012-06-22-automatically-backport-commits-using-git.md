---
created: 1340352892
layout: post
redirect_from:
- article/90/
- node/90/
- automatically-backport-commits-using-git/
title: Automatically backport commits using git
---
_For those who just want the result please scroll to the bottom, otherwise you can read the story._

I started looking into how to backport commits since I know how easily git forward ports commits and figured there had to be a better way then the tricks and manual methods recommended in [documentation](http://drupal.org/node/1538380) and various results from google. My original plan was to make a plugin that would re-roll patches that need forward porting automatically on [drupal.org](htt://drupal.org/) and if possible backport as well. Over the course of an hour and half I found a variety of snippets that got me part of the way there, developed a basic idea of how to accomplish backporting in git, and slowly refined it to built-in commands. Huge thanks to _cbreak_ and _FauxFaux_ in [#git](irc://freenode.net/#git) on [freenode](http://freenode.net)!

The basic idea I started from was to simply remove all the commits between the commit you want to patch on top of and the last commit in the tree. To test my approach I started by creating a simple situation representing _7.x_ -> _8.x_ using the following.

```bash
$ git checkout -b test 8.x
$ git mv core/CHANGELOG.txt core/CHANGELOG2.txt
$ git commit -m "move"

$ echo "hello world" > core/CHANGELOG2.txt
$ git commit -am "edit"
```

I then proceeded to remove the 2nd to last commit (_"move"_) to see if the last commit (_"edit"_) would be updated to edit _core/CHANGELOG.txt_ instead of _core/CHANGELOG2.txt_.

```bash
$ git rebase -i HEAD ~3 # commented out the "move" commit and saved
$ git show
```

To my delight the "edit" commit was indeed updated.

The next step was to figure out how to remove all the commits between the one to be backported and the last point at which _7.x_ and _8.x_ "last overlap" or at the point _8.x_ was branched (those are not necessarily the same and in Drupal's case they are not). I first envisioned dumping _git rebase -i_ to a file, editing with script to remove commits and then rebasing. To that end it was suggested to change the _GIT_EDITOR_ environment variable to a _grep_ command and then use _git rebase -i_ which would work, but the better approach is to use _--onto_ which does exactly that.

```bash
$ git rebase --onto=A B
```

In simplest terms: remove all commits between A and B non-inclusively.

Now I simply needed a way to find the best point to merge onto. There are several snippets in stackexchange and elsewhere that find the "oldest ancestor" which would work, but a) are not optimal, and b) require long snippets that are not easy to understand. When I dug around in _#git_ I was pointed to _git merge-base_ which finds the best point for such an operation.

```bash
$ git merge-base 7.x 8.x
```

Finds the last common commit between the _7.x_ and _8.x_ branches. Using this we get the following.

```bash
$ git rebase --onto=`git merge-base 7.x 8.x` HEAD~1
```

Backport the latest commit onto the _merge-base_ for _7.x_ and _8.x_. Finally, we just need to forward port the patch to the end of the _7.x_ branch which is easy.

```bash
$ git rebase 7.x
```

## Result

So putting it all together we get the following powerful one-liner that will backport the last commit in the _8.x_ branch to the _7.x_ branch.

```bash
$ git rebase --onto=`git merge-base 7.x 8.x` HEAD~1 && git rebase 7.x
```

If you want to backport something other than the last commit simply checkout a branch with that commit as the HEAD.

```bash
$ git checkout -b backport-branch commit-to-backport
```

This can also be extended to backport a patch straight on drupal.org by downloading and committing the patch first.

```bash
$ your facorite download method (wget, curl, etc)
$ git apply some.patch
$ git commit -am "patch to backport" 
$ git rebase --onto=`git merge-base 7.x 8.x` HEAD~1 && git rebase 7.x
$ git show > some.patch
```

You can of course use a fancier _git format-patch_ and what not, but this gets the point across. Enjoy!
