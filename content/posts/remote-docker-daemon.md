---
title: "connecting to a remote Docker daemon over SSH"
description: ""
date: 2022-12-26T08:15:15-05:00
draft: false
tags:
- docker
- ssh
---

Recently, I was working on a project that required connecting to a Docker daemon running on a remote machine. 
This is a common use case for Docker, and there are a few ways to accomplish it. I will describe the method I used.

## motivation
Typically, I use Docker on my local machine, but this project upended that workflow. 

For my use-case, I needed to interact with a remote daemon because:
* The remote daemon had containers running in a [bridge docker network](https://docs.docker.com/network/bridge/), which was not exposed to the local network.
* I could not restart the remote daemon with the [`--host`](https://docs.docker.com/config/daemon/) flag to expose the daemon to the network because of a long-running process that I did not want to interrupt.

A side benefit of this approach was that I was able to use my [local tooling](/posts/docker-fish-abbreviations/), including my IDE, to interact with the remote daemon. This enabled a faster feedback loop as the remote machine was fairly bare-bones.

## the plan
The remote docker daemon runs using a Unix domain socket by default. As mentioned, restarting the demon was not an option.

Thus, the rough plan was to:
1. Start an SSH tunnel to the remote machine.
2. Instruct my local Docker CLI to use the SSH tunnel as a proxy. 

## the solution
First, I needed to establish an SSH tunnel to the remote machine. 

I ran into a few issues doing so, but finally found that the following options needed to be enabled in `sshd_config` on the remote machine:

```shell
AllowTcpForwarding yes
GatewayPorts yes
```
I restarted the remote/server SSH daemon after making these changes to `sshd_config`.

Then, I established the SSH tunnel with the following command (on the client):

```shell
ssh -S none -N -L 2379:/var/run/docker.sock user@remote-machine
```
* The `-S none` flag is used to disable the use of a `ControlPath`, which is not needed in this case, and was causing problems, as I tried to use the same SSH tunnel for the SSH tunnel _and_ an interactive session [^1].
* The `-N` flag is used to disable the execution of a remote command; essentially make the session non-interactive, as I would be interacting with the daemon via the Docker CLI.
* The `-L` flag is used to specify the local port to forward to the remote socket. In this case, I used port `2379` because it is not a default port for Docker, and thus less likely to conflict with other services.

Finally, I configured the Docker CLI to use the SSH tunnel as a proxy:

Bash/Zsh:
```shell
export DOCKER_HOST=localhost:2379
```

Fish:
```shell
set -gx DOCKER_HOST localhost:2379
```

To sanity check the connection, I ran the following command:

```shell
docker ps
```

## future improvements
This workflow is good but not *great*, as it requires two steps, and some context-switching (docker and ssh).
In the future, I want to use [docker contexts](https://docs.docker.com/engine/security/protect-access/) to achieve the same workflow. I could not get this to work out of the box, and I suspect it has to do with reading my SSH configuration.

[^1]: I believe this conflict can be avoided by setting `ControlPath` in my ssh config more appropriately. Presently, my setting consists of only the server address `/tmp/ssh-%r@%h:%p`, so different commands (eg `ssh` and `docker` ) would share the same connection.
