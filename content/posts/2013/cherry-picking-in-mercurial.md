---
title: "Cherry picking in mercurial"
description: "Cherry picking (aka grafting) in mercurial."
date: 2013-03-28
tags: ["Mercurial", "hg"]
---
Quick side note today. I recently had managed to check a bunch of changes into the development branch that were ment to go in a release branch. If i could merge the develop into the release branch, that would have been an easy fix, but in this case new development is going on in that branch that cannot be put into the release branch.

**[Mercurial graft](http://www.selenic.com/mercurial/hg.1.html#graft) to the rescue.**

In my case i had a single revision that i wanted to cherry pick from the development branch into my release branch.

## Example
```console
hg up release-x.y.z
hg graft 12345
```

## Resources
<http://www.selenic.com/mercurial/hg.1.html#graft>