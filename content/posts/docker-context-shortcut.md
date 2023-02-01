---
title: "shortcut for using docker contexts"
description: ""
date: 2023-01-05T08:13:00-05:00
draft: false
tags:
- docker
- fish
---

As cool as [docker contexts](/posts/remote-docker-daemon-pt-2) are, I kept running into an issue: I would forget to switch contexts before running a command. I wanted to be able to run `docker` commands as normal, but have them automatically use the correct context.

I decided to devise a solution consisting of convention and my existing tools:
* I name my directories containing `docker-compose.yml` manifests after the context they are intended for.
* I use command substitution when executing `docker` commands, so that I can use the current directory name to determine the context.

As an example, suppose I want to use a docker context called `remote-alpha`. I would create a directory called `remote-alpha` and place a `docker-compose.yml` file in it. Then, I would run the following command:

```shell
docker --context (basename (pwd)) compose up -d --build
```

This command will use the current directory name to determine the context, and then execute the `docker compose` command with the `--context` flag set to the current directory name.

For non-fish users, this is equivalent to:

```shell
docker --context $(basename $(pwd)) compose up -d --build
```

In the future, I may create a fish function to encapsulate this logic, but for now I am happy with the convention and command substitution approach, as it is easy to modify and extend. I may also try my hand at supporting [completions](https://fishshell.com/docs/current/completions.html) for docker contexts, which thankfully fish makes very easy. 
