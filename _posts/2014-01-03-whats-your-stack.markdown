---
layout: post
title: "What's your stack?"
date: 2014-01-03 22:09
comments: true
categories: stack ruby ember digital-ocean chef
---

I'll describe mine for the project I'm working on currently. I've started with
[Ruby 2.1.0](https://www.ruby-lang.org/en/news/2013/12/25/ruby-2-1-0-is-released/)
and [Rails 4.1.0](http://weblog.rubyonrails.org/2013/12/18/Rails-4-1-beta1/).

For hosting I'm using [Digital Ocean](https://www.digitalocean.com/?refcode=e9f6ba3028c2)
which is really great service for the price. I'm also a big believer in
[Immutable infrastructure](http://chadfowler.com/blog/2013/06/23/immutable-deployments/)
philosophy. To get closer to it, I'm using [Vagrant](http://www.vagrantup.com) to build
my server with [Chef Solo](http://docs.vagrantup.com/v2/provisioning/chef_solo.html).
After I'm finished with server configuration, I can easily build same server on
Digital Ocean VPS with great [Vagrant Digital Ocean plugin](https://github.com/smdahlen/vagrant-digitalocean).

For the front end part I'm a fan of [Ember.js](http://emberjs.com/) MVC framework.
To get it working seamless with Rails I'm using [ember-rails](https://github.com/emberjs/ember-rails) gem.

This is my current stack. What's yours?
