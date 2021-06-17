---
title: "a few handy git abbreviations"
date: 2021-06-11T07:58:23-04:00
draft: true
tags:
- fish
- git
---

## summary

During the [inner loop of development](https://www.getambassador.io/docs/telepresence/latest/concepts/devloop/#what-is-the-inner-dev-loop), I often find myself repeating the same cumbersome `git` commands. 
These commands typically facilitate building and validating software changes or ensuring I am collaborating effectively with other teammates. Below I describe a handful of my favorite [fish shell abbreviations](/posts/docker-fish-abbreviations/) that maximize my git productivity.

## "just push my code"

Sometimes I want to push my changes quickly and often. The goal is usually to offload some relatively expensive work to a continuous integration server, instead of getting bogged down on my development machine.

For example, a change may be validated on my machine, but running a full test suite remotely may be desired. I have been fortunate enough to be on teams with extensive end-to-end test suites, but those suites can sometimes take hours if run locally.  

For such times, I use the following abbreviation:

```shell
abbr -a just-do-it 'git commit -a --amend --no-edit; git push --force-with-lease'
```

This abbreviation accomplishes the following:
* Commits all changed tracked files via the `-a` flag.
* Amends the previous commit via `--amend`, so that git does not prompt for a new commit.
  * Reuses the same commit message via `--no-edit`.
* [Force pushes with lease](https://git-scm.com/docs/git-push#Documentation/git-push.txt---force-with-leaseltrefnamegtltexpectgt), a safer alternative to force pushing.
  * It will not overwrite someone else's commit.
  * It will fail if there were changes in the remote branch (e.g. rebase).
* It references [multiple](https://en.meming.world/wiki/Thanos%27_«Just_Do_It») [memes](https://knowyourmeme.com/memes/shia-labeoufs-intense-motivational-speech-just-do-it), so it is fun to use on a daily basis.

This combination of flags and commands is very powerful, so it should be used responsibly. For example, it is often undesirable to overwrite commits, and [atomic commits](https://sparkbox.com/foundry/atomic_commits_with_git) should be preferred.

## "update my branch"
When working on a long-lived feature branch, it is often helpful to keep up-to-date with the main branch. The following one-liner typically satisfies that need in my workflow:
```shell
git pull --autostash --rebase origin main
```
This command is very handy because in one line you get the following:
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