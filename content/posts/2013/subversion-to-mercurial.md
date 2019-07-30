---
title: "Move from subversion to mercurial"
description: "Converting a subversion repository into mercurial."
date: 2013-05-08
tags: ["Mercurial", "hg", "Subversion", "svn"]
---
I just revisited an older project and did frankly not look forward to working with subversion again. Especially after just finishing a 4 month project where we have been working in mercurial, taking advantage of all the nice things a DVCS can give you. So i decided to try and move the repository to mercurial to enable a more modern development approach for myself and others working on this project.

## Here's what i did:
- Create a local svn repository (to be used as a mirror of the remote repository).
- Create a pre-revprop-change hook script in your local repository:
    - Create a new file named pre-revprop-change.bat
    - Add the following line exit 0 and save the file.
    - Put the file in the hooks directory in the repository folder.
- `svnsync initialize file:///C:/Develop/Path/To/Your/Mirror-Repo "https://external/svn/repo/"`
- `svnsync synchronize file:///C:/Develop/Path/To/Your/Mirror-Repo`
- Sit back and wait for the synchronization
- Add the following to your mercurial.ini (to enable the mercurial convert extension):
```
[extensions]
hgext.convert =
```
- Optional: Setup custom trunk/branches/tags paths in mercurial.ini (can also be done in the convert command)
```
[convert]
svn.trunk=Customer Specific/Customer/trunk
svn.branches=Customer Specific/Customer/branches
svn.tags=Customer Specific/Customer/tags
```
- Optional: Write an authormap file to map usernames in svn to hg style user names
- Optional: Write a branchmap file to map branch names (for instance trunk to default)
- `hg convert --authormap .\authormap.txt --branchmap .\branchmap.txt --datesort .\Mirror-Repo .\Hg-Repo`
- You're done!

## Resources
<http://mercurial.selenic.com/wiki/ConvertExtension>
<http://svnbook.red-bean.com/en/1.5/svn.ref.svnsync.c.init.html>
<http://stackoverflow.com/questions/3061259/how-to-backup-a-remote-svn-repo-while-you-have-not-had-admin-rights>