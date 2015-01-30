---
layout: post
title: "Don't download SVGs, style them"
date: 2015-01-30
categories: svg css
---

SVG (Scalable Vector Graphics) are great for the [internet of today](http://caniuse.com/#feat=svg). Check out
[this amazing presentation about them](https://docs.google.com/presentation/d/1CNQLbqC0krocy_fZrM5fZ-YmQ2JgEADRh3qR6RbOOGk/preview?slide=id.p),
if you have any doubts.

## Problem

Referencing SVGs with file path in CSS and/or HTML files will spawn additional HTTP requests when loading a page.
Including SVGs inside CSS will prevent you from styling them.

## Solution

We can define SVG elements in separate template, so they won't clutter other templates. Now we can reference them
with `use` tag.

- `icons.handlebars` or whatever templating language you're using, for defining symbols

{% highlight html %}
<svg class="svg-icons" display="none" version="1.1" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink">
  <defs>
    <symbol id="icon-three-dots" viewBox="0 0 18 4">
      <path class="path" fill-rule="evenodd" clip-rule="evenodd" d="M0,2c0,1.1,0.9,2,2,2s2-0.9,2-2S3.1,0,2,0S0,0.9,0,2z M7,2c0,1.1,0.9,2,2,2
        s2-0.9,2-2s-0.9-2-2-2S7,0.9,7,2z M14,2c0,1.1,0.9,2,2,2s2-0.9,2-2s-0.9-2-2-2S14,0.9,14,2z"/>
      </symbol>
      <!-- more icons/SVGs with own id's here -->
  </defs>
</svg>
{% endhighlight %}

- referencing

{% highlight html %}
<svg class="icon"><use xlink:href="#icon-three-dots"></use></svg>
{% endhighlight %}

Be sure to include `icons.handlebars` into your DOM before referencing them. Now we avoided doing
unnecessary requests and because icons are part of the DOM, we can style them with CSS. Double win!

[A fine color trick](http://css-tricks.com/cascading-svg-fill-color/)

{% highlight css %}
.icon { fill: currentColor; }
{% endhighlight %}

This will color SVG's fill with the same color that's set on the element that wraps SVG.

Thanks to my coworker Gregor who found most of these things out.
