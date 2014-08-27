---
layout: post
title: "Keep your servers DRY with Chef"
date: 2014-01-05 17:44
comments: true
categories: chef puppet vagrant digital-ocean
---

I'm not good with managing servers. Every time I needed to set up new production
server I was going through same resources on the internet; copy - pasting
commands into terminal. There must be a better way.

I first tried `Puppet` with `Vagrant` for building development environment in a
virtual machine. But I didn't like git submodules for managing puppet components
. I know there are better options, but I was using this for some time.
Then I saw video from [Jamie Winsor](http://www.youtube.com/watch?v=hYt0E84kYUI)
about [Berkshelf](http://berkshelf.com/). I was sold. In my head this clicked
like using `Rubygems` for building servers.

Fast forward. I've just pushed first commits of
[Digital Ocean Ruby cookbook](https://github.com/zigomir/do_ruby). It will let
you create and provision a `Droplet` on Digital Ocean's cloud as well on your
local machine with help of `Vagrant` and `VirtualBox`.

In this specific cookbook written with Chef you'll get `rbenv` which will build
and install Ruby version that you want, newest `Postgres` database and newest
`node.js`. All this will be running on Ubuntu 12.04 64.
Cookbook is in very early stage at the moment and I'll add more options to it
later on. For more information just check out the
[README](https://github.com/zigomir/do_ruby/blob/master/README.md).

With it I can build new server for any project with just a few commands in a
terminal. Isn't that sweet?
