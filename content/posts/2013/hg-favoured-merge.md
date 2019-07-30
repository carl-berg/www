---
title: "Mercurial favoured merge"
description: "A merge where you know which branch is 'right'"
date: 2013-10-17
tags: ["Mercurial", "hg"]
---

Sometimes you make a merge, and you know that one of the branches is "the right one", but still you have to specify how to merge each merge conflict. This is a handy way of specifying which branch should be used when conflicts arise:

```console
hg merge --tool internal:other
hg merge --tool internal:local
```
With other, the branch you are merging from is favoured, with local, the branch you are currently in is favoured.

## Resources
<http://stackoverflow.com/questions/15269060/mercurial-force-merge-branch>