---
title: "git with ssh on Windows 10"
description: "Configuring git ssh access for bitbucket"
date: 2020-02-16
tags: ["git", "ssh", "Windows 10", "bitbucket"]
---

I just repaved my home computer and though lost my old bitbucket ssh setup. Luckily Windows 10 now has a built in ssh agent so this is what i did:

```powershell
> cd  ~\.ssh
ssh-keygen -t rsa -C "carl@carl-berg.se"
```

Create a new file `~\.ssh\config`:
```
Host bitbucket.org
  IdentityFile ~/.ssh/bitbucket
```
... so that bitbucket knows which ssh key to use.

And we're ready to connect!  
`git clone git@bitbucket.org:my_user_name/my_repo.git`

## Resources

- [SSH config](https://stackoverflow.com/questions/17643855/cant-push-to-bitbucket-permission-denied-publickey#answer-23731813)

- [Add your key to bitucket](https://confluence.atlassian.com/bitbucket/set-up-an-ssh-key-728138079.html#SetupanSSHkey-Step5.AddthepublickeytoyourBitbucketsettings)