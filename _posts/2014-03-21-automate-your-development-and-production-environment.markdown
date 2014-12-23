---
layout: post
title: "Automate your development and production environment"
date: 2014-03-23 21:16
comments: true
categories: vagrant ruby digital-ocean
---

In this post I will describe how can you automate your development and production environment. It's an extension of my previous post [Keep your servers dry with Chef](http://zigomir.github.io/blog/2014/01/05/keep-your-servers-dry-with-chef/).

### Some background

I started working on side projects for fun around 2009. It was because of [Google App Engine](https://developers.google.com/appengine/). The most important thing for me, was the ability to put my application online for free, without a hassle. At that time I didn't know anything about configuring Linux servers. GAE was really good starting point at that time. These days there are a lot of easy to start providers. Most popular is probably [Heroku](https://www.heroku.com/).

### Chef + Vagrant + Digital Ocean = development + production environment

Fast forward 2014.

Why put in all the effort and not just spin up GAE or Heroku?

Well...I don't like to customize my code base for any specific provider. This is not so true for Heroku, but they are quite expensive when you want constant ping times. This is why I like [Digital Ocean](https://www.digitalocean.com/). They provide vanilla VPS server for as little as 5$ per month. They also have great APIs that will let you automate your server configuration with the tools I'll describe later.


I'm not a `sys admin` but I can do a basic setup that will be able to serve a simple application.

After configuring few servers I immediately felt the pain of repeating myself. Tasks such as configuring web server, installing database and Ruby/Python/Node is boring.

I'm a big [Vagrant](http://www.vagrantup.com/) fan. But Vagrant by itself won't let you script your environment. This is where Chef comes in. I learned most of it through this great [blog post by Mischa Taylor](http://misheska.com/blog/2013/06/16/getting-started-writing-chef-cookbooks-the-berkshelf-way/).

After you have your development environment scripted with Chef (or Puppet, etc.) you can easily build the same box on Digital Ocean and use it for production. All you need is to use [vagrant-digitalocean](https://github.com/smdahlen/vagrant-digitalocean) plugin. There is an [AWS plugin](https://github.com/mitchellh/vagrant-aws) too, but I haven't tried it yet.

When you have your environment setup (Chef, Puppet, ...) scripts done, you're able to say:

- `vagrant up` which will create a new VirtualBox machine for development environment
- `vagrant up --provider=digital_ocean` which will create same environment as a new Digital Ocean Droplet

By the way, these scripts, are meant to be saved in your source code repository. This goes hand in hand with philosophy of `immutable infrastructure`. Good post on this subject was written by [Chad Fowler](http://chadfowler.com/blog/2013/06/23/immutable-deployments/).

### Where do I start?

You can fork or take a look at my example on how to do this for
[Ruby/Rails/Postgres/Passenger/Apache environment](https://github.com/zigomir/do_ruby).

I hope this might help you to get your application from an idea to publicly available service. Publishing my applications online is what gave me most of motivation back in my `Google App Engine` days.

If you have any problems with this approach just ping me on [Twitter](https://twitter.com/zigomir) and I'll be happy to help you out.
