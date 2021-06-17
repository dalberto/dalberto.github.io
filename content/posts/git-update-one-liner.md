---
title: "my favorite one-liner to update a local git branch"
date: 2021-06-17T10:54:14.974615+00:00
draft: false
tags:
- git
---

When working on a long-lived feature branch, it is often helpful to keep up-to-date with the main branch. The following one-liner typically satisfies that need in my workflow:

```shell
git pull --autostash --rebase origin main
```
This command is very handy because it:
* Pulls the remote branch.
* Rebases your changes on top of the `main` branch.
* Stashes any local changes you may not have committed yet.

The equivalent consists of the following commands:
```shell
git pull origin main
git stash
git rebase main
```

While executing these commands one by one is not particularly onerous, they require some context-switching that may needlessly disrupt [flow](https://en.wikipedia.org/wiki/Flow_(psychology)).