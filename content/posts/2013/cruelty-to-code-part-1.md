---
title: "Cruelty to code"
description: "When digging through your old code you always find a few gems (atrocities)."
date: 2013-06-10
tags: ["C#"]
---
When digging through old code its only a matter of time before you find something nasty. Today i found this gem:
```cs
provider.UpdateOrderStatus(newStatusId, order.Id, 0, 0, 0, 0, 0);
```
First of all the provider is basically a repository that contains access to everything in the database. Secondly, who knows what the zeros mean. Could i use -1? no way to know without digging all the way down and checking.

If you create a method like this. Please rethink. 