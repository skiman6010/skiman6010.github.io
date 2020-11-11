---
layout: post
title: Preparing for GitHub Main Default
---
## Changing existing branches
As we prepare for GitHub to completely switch over to `main` as the default branch, I have been doing some cleanup on projects I have not yet pushed. GitHub is working on a tool to transfer over already pushed repos [by the end of the year](https://github.com/github/renaming#later-this-year). That tool will "seamlessly" transfer open PRs, draft PRs and branch protection policies. I'm impatient and want to get into the habit of switching to the new naming scheme now, so here is a braindump of things I have done so far.

    git branch -m master main

> `-m` keeps history so everything moves over to this new branch! You'll be able to `git reflog` and `git blame` with no issues just like before.

    git push -u origin main

> Push and change the tracking branch over to `main`.

___

### Fix up local clones

[https://twitter.com/xunit/status/1269881005877256192](https://twitter.com/xunit/status/1269881005877256192)

    $ git checkout master
    $ git branch -m master main
    $ git fetch
    $ git branch --unset-upstream
    $ git branch -u origin/main
    $ git symbolic-ref refs/remotes/origin/HEAD refs/remotes/origin/main
___

## Preparing for the future
As of git 2.28 there's a new config option to set the default branch. How nice of them!

    git config --global init.defaultBranch main

This config will tell `git init` to default to `main` as the naming scheme, but you can change that to `trunk` or `latest` if you prefer SVN esque naming schemes.

I run an Ubuntu derivative and my `apt` was on git `2.17.X` so I just went ahead and ran

    add-apt-repository ppa:git-core/ppa

and then 

`sudo apt update` and `sudo apt upgrade` to install the newest version of `git`.
