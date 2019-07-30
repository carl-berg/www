---
title: "Using git with a subversion repository"
description: "Concluding my first weeks of using git-svn"
date: 2012-10-04
tags: ["Git", "Svn"]
---

I have been working with a few different source control systems over the years. To start with we used Visual Source Safe 2005, which I found to be clunky but it worked (apart from the usual walks around the office asking people to check in the file you need). We later on switched to Subversion and this was when i felt like using a source control system was actually something that could help me in my work, not just be a vault with backup for all the source code.

I have been using Subversion for a few years now and i like it, but it has some drawbacks. It can sometimes get into weird states where you cant check in or out files because of some mysterious errors. After a while you get used to the clunkyness and learn to to "trick" Subversion (like removing folders and updating). Subversion also intoduces some friction when you're working in teams and need to merge often (i would say both Git and Mercurial are better at merging), also branching works, but it's not nearly as easy as with Git or Mercurial.

During the last couple of years i have tried out Git and Mercurial and i have to say i like them both. Mercurial feels a bit "safer", coming from a windows background, because the gui tooling is alot better (like [tortoisehg](http://tortoisehg.bitbucket.org/)), although there are things happening in the git for windows space aswell (like [github for windows](http://windows.github.com/)). I feel that the Git community is a little more vibrant with sites like github and even azure now supports deployment through Git.

My collegues seem quite happy to keep using Subversion though, so in order to not move anyones cheeze i decided to try out git-svn. Here comes a little recap of useful things i have learned during the last couple of weeks of using git-svn. 

## My .gitattributes file:
This is the configuration that caused me the lease grief with an existing Subversion repository. Is basically means, do no line ending transformations at all. This might not be what you want in a "proper" git repo, but for git-svn this worked the best for me.
```
* -text
```

## Some useful git-svn commands
These are some useful git-svn commands i have used in my recent day to day work.

```console
> git svn clone -s https://url/to/your/svn/repo // -s indicates standard structure with trunk, branches and tags
> git svn rebase //fetches updates from your svn repo 
> git svn dcommit //commits changes made to your local git repo
> git svn fetch //fetches changes in repo like new branches etc
```

## Some useful Git commands
```console
git checkout -b new-local-branch svn-branch //creates a new local branch synced with remote svn-branch
```

## Caveat on merging
When using Git branches synced with remote svn branches, one important detail when merging a branch like that is to use the --no-ff flag to prevent Git from using a fast forward merge. See the stack overflow link below if you want to read more about my perticular problem i had with this.
```console
git merge local-branch --no-ff //use the no-ff flag to prevent fast forward merges
```

## Conclusion
Overall it seems to work pretty well. Syncing with Subversion is a little slow, but i don't do that all the time. One thing to note is that it doesn't handle externals, but i'm not sure i can say that subversion handles externals all that well either. I might add some more information in this post if i find out anything else worth writing about.

## Resources
- <http://viget.com/extend/effectively-using-git-with-subversion>
- <http://www.jukie.net/~bart/blog/svn-branches-in-git>
- <http://stackoverflow.com/questions/12605767/git-svn-merging-and-committing-branches>