---
layout: post
title: "uploaded my dotfiles again"
tags: dotfiles
---

I've played this game before.  I uploaded my dotfiles to github
and failed.

This time is different.  You can find them
[here](https://github.com/dlamotte/dotfiles).  This time, instead of
a horribly hacked Makefile installing the files, I've gone with symlinks
which seems that others have been doing successfully.

But I am not using `lndir` because it doesn't quite work the way I'd like.
In some cases, I want the whole directory to be symlinked.  In others, I want
subfiles symlinked and I want fine grained control of that (but I want good
defaults so adding new files isn't a hassle).  On top of this, I hate the idea
of symlinking the dotfiles straight across.  How ugly.  I don't want to have
to `ls -a` in my sandbox of dotfiles all the time.  The "install script" should
be smart enough to prefix symlinked files with a `.` so that I can look at the
files in the dotfiles dir "normally".

These two factors made me decide to write the script you'll find in there
called `lndotfiles.py`.  This script has some global variables that act like
configuration and also does the magic explained above.

I also have the distinct problem of having separate dotfiles between my work
computer and my home computer.  I've (hopefully) solved this problem this time
around by adding a `~/.home/` and `~/.work/` directories which will contain
location specific config files that will get sourced by the associated config
file via `~/.current/...` where `~/.current` is a symlink to the appropriate
directory.

Another interesting extension is the `alias` I've added to my
`~/.current/zshrc`.  If I'm editting dotfiles in my home directory, I
shouldn't need to `cd` into my dotfiles sandbox.  Instead, I should have
direct access to the dotfiles sandbox.  The `alias` looks like this:

    alias gitdot="git --git-dir=$HOME/sandbox/dotfiles/.git --work-tree=$HOME/sandbox/dotfiles/"

Now I can just use `gitdot` in place of `git` and work on my dotfiles from
wherever I like.
