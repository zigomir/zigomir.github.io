---
layout: post
title: "Ember on fire(base)"
date: 2014-02-09 21:12
comments: true
categories: ember.js javascript firebase baas noBackend real-time
no_turbolinks: true
---

In this post I'll show you how to create a simple Ember TODO™ app
that saves data in Firebase. Later on I'll write down some thoughts
about this approach and it's limitations.

## A Happy TODO™ app

I always enjoyed playing with anything real time. In past I used
[faye](http://faye.jcoglan.com/). It worked out just fine. But to use
it you need to run another Rack/Node server (I was using Ruby version)
and Redis database on which Faye depends. This complicates your
system a little bit. I used it somewhere between 2011 and 2012.

Fast forward 2014 and we have backend services like
[Firebase](https://www.firebase.com).

For Ember app you can now easily use Firebase with [emberFire](https://github.com/firebase/emberFire) library.
I've (re)written an example app that uses Ember and Ember Data.

<a class="jsbin-embed" href="http://jsbin.com/kovoc/3/embed?html,js">Fire Ember</a><script src="http://static.jsbin.com/js/embed.js"></script>

As you can see, there is not much code. If you open it in another browser you'll
see that new todos will be automagically added in real time. This was easy and fun
to write.


## Concerns...

Of course there are downsides to this approach. I'll ignore pricing for now.

First one that came to my mind was of course a lack of control on server side. I
Googled for sending an email from Firebase and didn't got many answers. There was
[zapier](https://zapier.com/) integration with [SendGrid](http://sendgrid.com).
So here we are, adding 3rd party service dependencies like a maniacs. If one is down,
our app is down or not working consistently. This is a bad sign.

Then there was this really simple feature that I wanted to solve on server side -
ordering. I did a really quick lookup through the
[Firebase docs](https://www.firebase.com/docs/) and found a way to sort the data
with `.priority` attribute. This feature doesn't seem to be supported in
`emberFire` yet, so I decided to solve it on client side with adding `createdAt`
attribute. I was a little shocked at first that I couldn't do something I'm so
used to.

At the end I think you can still do pretty useful applications with this approach.
But you need to have a really clear feature list at the very beginning. Because with
growth you'll probably get bitten with all the dependencies and a lack of control
on server side.

## Adding hacks?

I like using Ruby on server side. And Firebase of course has a
[Ruby client](https://github.com/oscardelben/firebase-ruby). Then I thought you could
write a Ruby app that runs and listens to Firebase events and responds to them.
Let me write that more clearly in an example.

Let's say we'll write a Ruby program that runs on a server which connects to the same
Firebase as your client app. Then you should be able to receive messages from Firebase
in a pub(lish) / sub(scribe) fashion and respond with arbitrary actions,
like sending an email or whatever.

Unfortunately this isn't supported with current `firebase-ruby` client library.
It would be an interesting approach...
