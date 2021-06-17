---
title: "my favorite git command for pushing changes fast"
date: 2021-06-11T07:58:23-04:00
draft: true
tags:
- git
---

Sometimes I want to push my changes quickly and often. The goal is usually to offload some relatively expensive work to a continuous integration server, instead of getting bogged down on my development machine.

For example, a change may be validated on my machine, but running a full test suite remotely may be desired. I have been fortunate enough to be on teams with extensive end-to-end test suites, but those suites can sometimes take hours if run locally.  

For such times, I use the following one-liner:

```shell
git commit -a --amend --no-edit; git push --force-with-lease
```

Technically, it is two lines in raw shell form, but I use an [abbreviation](/posts/docker-fish-abbreviations/) to get around that. 

This command accomplishes the following:
* Commits all changed tracked files via the `-a` flag.
* Amends the previous commit via `--amend`, so that git does not prompt for a new commit.
  * Reuses the same commit message via `--no-edit`.
* [Force pushes with lease](https://git-scm.com/docs/git-push#Documentation/git-push.txt---force-with-leaseltrefnamegtltexpectgt), a safer alternative to force pushing.
  * It will not overwrite someone else's commit.
  * It will fail if there were changes in the remote branch (e.g. rebase).

This combination of flags and commands is very powerful, so it should be used responsibly. Specifically, it is often undesirable to overwrite commits, and [atomic commits](https://sparkbox.com/foundry/atomic_commits_with_git) should be preferred.
