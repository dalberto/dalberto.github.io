---
# title: "Using fish shell abbreviations to level up Docker commands"
#title: "Power up your Docker CLI with fish abbreviations"
# title: "Take the Docker CLI to 11 with fish abbreviations"
# title: "Take the Docker CLI to 11 with fish abbreviations"
title: "power up docker cli with fish abbreviations"
description: ""
date: 2021-05-02T21:09:58-04:00
# showToc: true
draft: true
tags:
- fish
- docker
---

## summary
My favorite shell is [fish](https://fishshell.com), and one feature I find especially useful is [abbreviations](https://fishshell.com/docs/current/cmds/abbr.html).

Abbreviations are essentially text-expanded shortcuts. For example, a shortcut `gco` will expand to `git checkout`. 

## basic usage
The fish shell documentation covers [usage thoroughly](https://fishshell.com/docs/current/cmds/abbr.html#examples), but my basic usage of abbreviations consists of adding them and using them.

To add an abbreviation simply call `abbr -a WORD EXPANSION`, where `WORD` is the abbreviation and `EXPANSION` is the expanded command.
For example: `abbr -a gco git commit`.

To use it, simply type `gco`, followed by `Space` or `Enter`. Pressing `Space` will expand the abbreviation, which enables editing. 
Pressing `Enter` on the other hand, expands and immediately executes the command. 

## advantages
Abbreviations can be very useful because:
* They save time by reducing typing. Moreover, commands that do not require editing can be executed immediately by entering `Enter`.
* They are flexible. They expand before execution after pressing `Space`, so minor changes are trivial.
* They are easy to set up. They can be defined in the shell without creating new files or updating configuration.

These advantages make them feel like a cost-free abstraction; they faciliate what I'm already doing without introducing cognitive overhead.
On the other hand, abbreviations are not a replacement for functions because they do not take arguments and can obfuscate routines beyond a few lines of code.

## docker
These advantages make abbreviations especially useful for commands that require long and esoteric options, like Docker.

These are some of my most productive Docker abbreviations:

### dka
```shell
docker run --pull=always --volume (pwd):/opt/app -w /opt/app --rm -it
```
This command is designed to run ephemeral containers. I often use this to experiment with small code snippets or packages. 
This abbreviation assumes the target image will be appended by the user. This command features:
* Pulling the latest version of the image specified.
* Mounts the current directory into the container under `/opt/app`.
* Changes the working directory to `/opt/app` for convenience.
* Configures the container for interactive usage (e.g. a shell/interpreter).
* Removes the container after exit.




