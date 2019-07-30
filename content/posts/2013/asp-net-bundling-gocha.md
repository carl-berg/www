---
title: "ASP.NET Bundling Gotcha"
description: "Something to think about when bundling files with paths to other files."
date: 2013-10-21
tags: ["ASP.NET", "MVC"]
---

I recently spent quite a bit of time trying to figure out why the IIS wasn't serving woff, ttf or svg files. I would simply get a 404 error.

![bundling-problem symtom](/img/bundling-goof-up.png)

The reason for this is that this site (and not the other site where it all works), is running with compilation debug flag set to false, which means that instead of using all the individual files in the style bundle, it's all packaged up and served from the path that i have given to the bundle. In this case the path i had given to the bundle did not match where the files were actually located.

Some of the css files i am bundling have relative paths like this `../fonts/filename.ttf` which won't match if my bundling path doesn't have the right "depth", so that's something to think about when things are working locally, but not on the staging server.