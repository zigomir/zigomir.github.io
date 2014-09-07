---
layout: post
title: "Ember components"
date: 2014-01-30 15:05
categories: ember.js javascript components ember-components
---

You want a reusable component which supports auto-completion search on user's
public GitHub repositories.

We'll use [Twitter's typeahead.js library](http://twitter.github.io/typeahead.js/)
for auto-completion. To make it reusable we will do an `Ember component`.

{% raw %}
```
{{repository-finder user="zigomir"}}
```
{% endraw %}

Handlebars template for rendering this component only needs to render one input

{% highlight html %}
<input type="text" />
{% endhighlight %}

By convention we need to save the template in
`templates/components/repository-finder.hbs` or give it
`data-template-name="components/repository-finder"` if you're defining your
templates within HTML file.

But our component will also handle an action when user selects a value in
auto-completion list. So we need to specify it

## Triggering action from component

{% raw %}
```
{{repository-finder user="zigomir" action="showSelectedRepo"}}
```
{% endraw %}

With `user` attribute we pass a GitHub user whose repositories we want to search.
`action` attribute tells Ember which action in controller will catch an action
that we send from component.

First we need to trigger an action inside a component. This is done with
`triggerAction`. We want to trigger it when `typeahead` value was `autocompleted`
or `selected`:

`App.RepositoryFinderComponent`
{% highlight javascript %}
// omitted code
.on(
    'typeahead:autocompleted typeahead:selected',
    function(object, datum) {
      that.set('selectedValue', datum.name);
      that.triggerAction({action: 'action', target: that});
    }
  )
// omitted code
{% endhighlight %}

This will trigger action named `action` inside component. Sending action from
component to outside world is done with `sendAction`...

`App.RepositoryFinderComponent`
{% highlight javascript %}
// omitted code
actions: {
  action: function() {
    this.sendAction('action', this.get('selectedValue'));
  }
}
// omitted code
{% endhighlight %}

...which is then sent up and will be caught by action that we specified in
template as a component attribute. In our example it is `showSelectedRepo`

`App.ApplicationController`
{% highlight javascript %}
// omitted code
actions: {
  showSelectedRepo: function(value) {
    this.set('selectedRepo', value);
  }
}
// omitted code
{% endhighlight %}

In our controller we just do whatever we want with caught action and the values
that were propagated with it.

## Initializing jQuery plug-in

If you try to use `jQuery plug-in` within Ember views or components you need
to do it when Ember actually inserts a view / component element into the DOM.
Approach you're probably most used to, won't work in Ember

{% highlight javascript %}
$(function(){
  // your plug-in initialize code
});
{% endhighlight %}

You need to do it inside your view / component on `didInsertElement` event.

Whole `RepositoryFinderComponent` code looks like this

{% highlight javascript %}
App.RepositoryFinderComponent = Ember.Component.extend({
  onRenderComponent: function() {
    var self = this;
    var url = 'https://api.github.com/users/' + this.get('user') + '/repos';

    var $field = this.$('input').typeahead([
      {
        name: 'repos',
        valueKey: 'name',
        prefetch: url,
        remote: url
      }
    ]).on(
      'typeahead:autocompleted typeahead:selected',
      function(object, datum) {
        self.set('selectedValue', datum.name);
        self.triggerAction({action: 'action', target: self});
      }
    );
  }.on('didInsertElement'),

  actions: {
    action: function() {
      this.sendAction('action', this.get('selectedValue'));
    }
  }

});
{% endhighlight %}

Full example is available here

<a class="jsbin-embed" href="http://jsbin.com/oBABaBej/2/embed?html,js,output">
  Repository Finder Component</a>
<script src="http://static.jsbin.com/js/embed.js"></script>

I've learned most about Ember components on [tutsplus.com tutorial](http://net.tutsplus.com/tutorials/javascript-ajax/ember-components-a-deep-dive/).
