---
layout: post
title: "Dynamic Ember views"
date: 2014-01-24 23:10
comments: true
categories: ember ember-views
---

I needed to step out of conventional `ember way` of building an application.
We need to be able to render Ember view wherever in the DOM, not just in an
`{% raw %}{{outlet}}{% endraw %}` defined in Ember templates. I also didn't want to use Ember's
router because same reasons.

For that you'll need to use `Ember.ContainerView`. I found about it on
[forever getting better emberjs.com guide pages](http://emberjs.com/guides/views/manually-managing-view-hierarchy/). Although examples listed
there didn't quite help me with my situation. This is also why I'm writing this
post.

How I solved the problem for rendering views in arbitrary DOM element was
using `container.lookup` function.

{% highlight javascript %}
App.ApplicationView = Ember.ContainerView.extend({
    didInsertElement: function() {
        var documentView = this.container.lookup('view:document');
        documentView.appendTo('#document');
    }
});
{% endhighlight %}

`var document View = this.container.lookup('view:document');`
will instantiate `DocumentView`. (I think, but I'm not sure, it is some kind
of factory, calling a create on a view).

And now when you have a reference for a view you can append it to arbitrary
element in the DOM: `documentView.appendTo('#document');`

### Wiring Views and Controllers

All the wiring up between routes, views, templates and controllers is done
automatically if you follow the  `Ember way` of doing things.

When you do it like described above, without routes and instantiating your
views by hand, you'll need to do an extra step to wire views with controllers.

This is how you do it

{% highlight javascript %}
App.DocumentView = Ember.View.extend({
    templateName: 'document',
    fileType: 'document',

    init: function() {
        this._super();
        this.set('controller', App.DocumentController.create());
    }
});
{% endhighlight %}

Here I'm telling that `DocumentView` should be `controlled` by
`DocumentController`.

And don't forget. If you have lot's of duplication in your view (or controller)
code, you can always create a common view and then extend it. You don't have to
always extend `Ember.View`.
