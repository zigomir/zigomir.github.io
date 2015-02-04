---
layout: post
title: "Why I think React is better than Backbone"
date: 2015-02-04
categories: react versus backbone javascript
---

A fellow said that React doesn't look anything new to him. I'll try to explain
the differences on a fairly simple example
[I used before](http://blog.zigomir.com/react.js/jquery/2015/01/11/jquery-versus-react-thinking.html).

## Performance

Performance is much better with React. We're doing same number
of renders on both examples. Click on `Run with JS` on bottom right of each
`JSBin` example, and then click on the rendered bags. There are 5000 of them.
I suspect Backbone's bottleneck is coming from `underscore`s
building of strings and then shoving them into the DOM via `$.html`.
I am not really sure how React handles this example (we know it must update all non-selected
items class) and still preserve good performance. I'd guess it's because it
doesn't need to juggle with giant strings like Backbone.


### Backbone

<a class="jsbin-embed" href="http://jsbin.com/qaxasa/2/embed?js,console,output">Backbone example/a>
<script src="http://static.jsbin.com/js/embed.js"></script>

## Code reasoning

I find Backbone's code simple and readable. What I dislike about it

- putting view state into model's properties `show` and `class`. It's most straight
forward thing I could think of
- separation of technologies instead of concerns - underscore template
(click `HTML` tab on `JSBin` and close all others to see what's going on)
and Backbone View code is separated so we need to look at two files to know
what and how it will be rendered
- order of lines inside `handleItemClick` method is important. We need to save
`alreadyOpened` before we change all items to collapse them
- we need to know and tell Backbone that we want to do first change silently.
Otherwise we'd trigger a ton of re-renders and performance would be much worse
- code is mostly imperative

### React

<a class="jsbin-embed" href="http://jsbin.com/hotod/13/embed?js,console,output">React example</a>
<script src="http://static.jsbin.com/js/embed.js"></script>

How React is better

- only state we hold is one boolean (`expandedProductId`) and `Shop`
component holds (also encapsulates) it. In React applications you never ask for
state of another component
- for model we use plain JavaScript array
- code is even simpler and more declarative

## Conclusion

Example is extremely simple and doesn't help us see real benefit or comparison
for real projects. I actually think that with enough discipline even big Backbone
application can stay fairly simple and is easy enough to maintain. But I know
for a fact, it's not all roses.

Most hard problems comes because object state can be mutated from anywhere.
Also, Backbone doesn't help you with state separation. State is creeping into
models and views from many places. We are (at least I know I am) really bad
at holding the state of where changes might come from.

If you're a bit more interested in React now, I suggestion you to see these two
videos

- [Steven Luscher: Decomplexifying Code with React](https://www.youtube.com/watch?v=rI0GQc__0SM)
- [React.js Conf 2015 - Immutable Data and React](https://www.youtube.com/watch?v=I7IdS-PbEgI)
