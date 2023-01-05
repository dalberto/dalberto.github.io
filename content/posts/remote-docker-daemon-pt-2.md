---
title: "connecting to a remote Docker daemon over SSH pt2"
description: ""
date: 2023-01-04T22:01:12-05:00
draft: false
tags:
- docker
- ssh
---

I previously connected to a [remote docker daemon using an SSH tunnel](/posts/remote-docker-daemon) . While my solution worked, I wanted to explore a more elegant solution using [docker contexts](https://docs.docker.com/engine/security/protect-access/#use-ssh-to-protect-the-docker-daemon-socket).

I initially attempted to use docker contexts as-is:

```shell
docker context create remote --docker "host=ssh://user@remote-machine"
```

And then attempted to use it and perform a sanity check:

```shell
docker context use remote
docker ps
```

However, I received an error like the following:

```shell
# other output
sh: docker: command not found
```

After [some searching](https://askubuntu.com/a/1215027/503333), I was able to get to a solution.

First, I needed to set `PermitUserEnvironment` to `yes` in `sshd_config` on the remote machine. Then, I modified `~/.ssh/environment` on the remote machine to include my full `PATH` variable. Finally, I restarted the remote SSH daemon.

After these tweaks, the previous docker commands succeeded!

Now, when I want to connect to my remote daemon I simply run:

```shell
docker context use remote
```

And execute `docker` and `docker compose` commands as normal. 🚀

In the future, I will surface the current docker context in my shell prompt, so I can easily see which context I am using, similar to how many prompts surface `git` working state
.