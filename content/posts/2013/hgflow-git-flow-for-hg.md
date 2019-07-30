---
title: "Hgflow - Git flow tooling for mercurial"
description: "A quick getting started guide to hgflow which gives you gitflow tooling support for mercurial"
date: 2013-11-21
tags: ["Mercurial", "hg"]
---

Yesterday I went to see an former colleague, [Kristoffer Ahl](https://twitter.com/kristofferahl), holding a talk about git flow. The talk was very interesting since I have been using the git flow structure with mercurial for about a year. One thing that was really neat was the gitflow tooling. I had no idea that there was a nice toolkit for helping maintain the gitflow structure. Of course i had to see if there was something similar for mercurial, and of course there is.

It's called hgflow. I just tired it out quickly and decided to do a quick writeup of it. The gist of it is the following:

## Setup
```console
> hg clone https://bitbucket.org/yujiewu/hgflow hgflow //clones the code for hgflow
```

```
//Edit your mercurial.ini, ususally found at %UserProfile%\mercurial.ini
[extensions]
hgflow = C:\{Path to where you cloned hgflow}\hgflow.py
```

```console
> hg flow init //initialize hg flow for your repository
```

## Getting started
```console
> hg flow feature start feature-a //creates a new feature branch
```
Go about coding and commit as you go...
```console
> hg flow feature finish feature-a //closes a feature branch and merges it into develop
```
Go about coding and commit as you go...
```console
> hg flow release start 0.1 //creates a release branch
```
Stabilize your realease...
```console
> hg flow release finish 0.1 //tags release branch, closes it and merges it into develop and default
```
...and a release has been made! And if you have a look at your source tree, it would look somthing like this:

![hg flow demo](/img/hg-flow-demo.png)

I will go about testing this out from now on and will post any gotchas along the way!

#### Update 2013-11-22
Added setup instructions.

#### Update 2014-05-19 
The repository has moved. Urls in the post have been updated to the new repository. The structure of the new repository is slightly different, so make sure the path to the hgflow.py file is right in your mercurial extension.

## Resources
- [Hg flow](https://bitbucket.org/yujiewu/hgflow/wiki/Home)
- [Introduction](https://andy.mehalick.com/2011/12/24/an-introduction-to-hgflow)