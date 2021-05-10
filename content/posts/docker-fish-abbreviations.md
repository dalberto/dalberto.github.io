---
title: "a fish shell abbreviation that sparks joy"
description: ""
date: 2021-05-02T21:09:58-04:00
draft: false
tags:
- fish
- docker
---

## summary
My favorite command line shell is [fish](https://fishshell.com), and one feature I find especially useful is [abbreviations](https://fishshell.com/docs/current/cmds/abbr.html).

Abbreviations are essentially text-expanded shortcuts. Continue reading for a quick rundown. 

## basic usage
To add an abbreviation simply call `abbr -a WORD EXPANSION`, where `WORD` is the abbreviation and `EXPANSION` is the expanded command.
For example: `abbr -a gco git commit`.

To use it, simply type `gco`, followed by `Space` or `Enter`. 

## advantages
Abbreviations can be very useful because:
* They save time by reducing typing. Moreover, commands that do not require editing can be executed immediately by entering `Enter`.
* They are flexible. They expand before execution after pressing `Space`, so minor changes are trivial.
* They are easy to set up. They can be defined in the shell without creating new files or updating configuration.
* They are self-documenting. The exact command executed is displayed.

These advantages make them a cost-free abstraction, and a joy to use; they facilitate what I'm already doing without introducing overhead.

## my docker run abbreviation
Docker commands are great candidates for being abbreviated because of the long and esoteric options. The following is one of my favorite abbreviations:
```shell
abbr -a dka "docker run --pull=always --volume (pwd):/opt/app -w /opt/app --rm -it"
```
The abbreviated command is designed to run ephemeral containers. I often use this to experiment with small code snippets or packages. 

This abbreviation assumes the target image will be appended by the user. This command features:
* Pulling the latest version of the image specified.
* Mounts the current directory into the container under `/opt/app`[^1].
* Changes the working directory to `/opt/app` for convenience.
* Configures the container for interactive usage (e.g. a shell/interpreter).
* Removes the container after exit.

There's a lot going on there, but I just call `dka`, followed by the image, to execute this command. :sparkles: :cake: 

## conclusion
In the end, Fish abbreviations spark joy because they contribute to a faster feedback loop. I get a trove of functionality with minimal effort.

[^1]:
    The abbreviated command is in quotes to avoid evaluating `(pwd)` until the abbreviation is expanded. 
    Ordinarily, abbreviations do not require quotes.
