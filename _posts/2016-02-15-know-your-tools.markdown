---
layout: post
title: "Know your tools"
date: 2016-02-15
categories: javascript javascript-fatigue vue vue-cli
---

## Problem

Have you tried turning it...googling [react starter kit](https://www.google.si/search?q=react+starter+kit&oq=react+starter+kit&aqs=chrome..69i57j0l5.2209j0j7&sourceid=chrome&es_sm=91&ie=UTF-8#)?

Do you know how [ember-cli](http://ember-cli.com/) works? Do you really want or need bower?

Some people joke how setting up `grunt` or `gulp` takes a week or more. How many packages in `packages.json` are there that you see for the first time?

## Possible solution (that works for me at the moment)

1. Learn [Vue](http://vuejs.org/); read [guides](http://vuejs.org/guide/).

2. Implement component(s) in [codepen](http://codepen.io/) or [JS Bin](https://jsbin.com/?html,js). You can fork my [Vue Starter codepen](http://codepen.io/zigomir/pen/XXoZyp)

3. Generate app with [vue-cli](https://github.com/vuejs/vue-cli)

{% highlight bash %}
npm install -g vue-cli
vue init zigomir/vue-simple-template <prototype-name>
{% endhighlight %}

<p style="margin-left: 15px;">4. Copy CodePen component into your app. Viola.</p>
