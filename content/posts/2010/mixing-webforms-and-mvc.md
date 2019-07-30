---
title: "Mixing webforms and MVC"
date: 2010-07-10
---

Yesterday, I spent a couple of hours debugging a problem with MVC forms posting not working right. I had a few MVC pages with a webforms masterpage. The masterpage had a form tag (which is pretty common in webforms masterpages). However, this caused the problem I had with the MVC pages. Apparantly you can't nest forms. This made every first form in the MVC pages post to the index action, even if another action was specified, but the rest of the forms posted like they should. This is something to think about when you're mixing webforms and MVC pages with a common webforms masterpage.