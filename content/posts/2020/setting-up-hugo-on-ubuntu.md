---
title: "Setting up hugo on Ubuntu"
description: "Some instructions on setting up hugo on Ubuntu/WSL2"
date: 2020-02-16
tags: ["hugo", "wsl", "wsl2", "ubuntu"]
---

I just repaved my home computer and though I would write up instructions for myself on how to setup hugo (which i use to publish this blog) on WSL2.

I first investigated [apt-get](https://gohugo.io/getting-started/installing/#debian-and-ubuntu) for Ubuntu, but the version there seems old (0.40.1)...

I want a specific version (0.56.2) which is the one i use at netlify, which i use for publishing and hosting.

Insteady i opt to install via `deb` package, so i go to check out the [hugo github releases for my specific version](https://github.com/gohugoio/hugo/releases?after=v0.57.0).

I found [hugo_0.56.2_Linux-64bit.deb](https://github.com/gohugoio/hugo/releases/download/v0.56.2/hugo_0.56.2_Linux-64bit.deb), which i download.

I install the package `sudo dpkg -i hugo*.deb`

And we're ready to run `hugo serve --config config.toml,config.dev.toml`

## Resources

- [Hugo installation docs](https://gohugo.io/getting-started/installing/#debian-and-ubuntu)
- [Some good example instructions at Digital Ocan](https://www.digitalocean.com/community/tutorials/how-to-install-and-use-hugo-a-static-site-generator-on-ubuntu-14-04)