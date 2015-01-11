---
layout: post
title: "jQuery versus React.js thinking"
date: 2015-01-11
comments: true
categories: react.js jquery
---

I don't like versus debates per se. I think each reasonably successful
library or framework had good reasons (at the time being) to become so in the
first place. We will compare conceptual differences behind jQuery and React.

## Problem

List of items. Each item has details which are hidden by default. Whenever user
clicks on an item, item will expand and show it's details.
Other items will turn to grey. If we click same item again, it will collapse
and items will be in their initial state, collapsed and black. We can also just
switch from one expanded item to another, collapsing previous one, expanding new
one.

## Two solutions

### jQuery ([JSBin](http://jsbin.com/zunev/3/edit){:target="_blank"})

{% highlight javascript %}
$(function() {
  $('li').on('click', function(e) {
    var $clickedItemDetails = $(e.currentTarget).find('.details');
    var $allDetails = $('li .details');

    if ($clickedItemDetails.is(':hidden')) {
      $allDetails.hide().parent().removeClass('collapsed');
      $clickedItemDetails.show();
    } else {
      $clickedItemDetails.hide();
    }

    $allDetails.each(function(index, el) {
      if ($(el).is(':hidden')) {
        $(el).parent().addClass('collapsed');
      }
    });

    if ($('li.collapsed').length === $('li').length) {
      $('li').removeClass('collapsed');
    }
  });
});
{% endhighlight %}

Our data and it's state are not structured and are spread inside the DOM.
Separation between data, state and presentation is vague.

We're querying the data from the DOM with jQuery's selectors
(like `is(':hidden')` and `.find('.details')`).
Then we're changing state directly on the DOM with `hide()`, `show()`,
`addClass()` and `removeClass()` functions.

I've written this code first some time ago and coming back, I needed to wrap my
head around it to understand it again. Because features are so limited I could
refactor it without breaking. But this wouldn't scale nicely if we'd add more
complicated features to it.

### React ([JSBin](http://jsbin.com/hotod/12/edit){:target="_blank"})

{% highlight javascript %}
/** @jsx React.DOM */
var PRODUCTS = [
  {
    "id": 1, "name": "Bag of suck", "price": 100,
    "details": "You don't want to own this!"
  },
  {
    "id": 2, "name": "Bag of luck", "price": 200,
    "details": "You might want to own this!"
  },
  {
    "id": 3, "name": "Bag of fuck", "price": 300,
    "details": "You really want to own this!"
  }
];

var ItemsList = React.createClass({
  getInitialState: function() {
    return {
      expandedProductId: null
    };
  },

  handleProductClick: function(product) {
    var newSelectedProductId = product.id;

    if (this.state.expandedProductId === product.id) {
      newSelectedProductId = null;
    }

    this.setState({expandedProductId: newSelectedProductId});
  },

  render: function() {
    var self = this, noneSelected = this.state.expandedProductId === null;

    var products = PRODUCTS.map(function(product) {
      var details, isExpanded = self.state.expandedProductId === product.id;

      if (isExpanded) {
        details = <div>{product.details}</div>;
      }

      return (
        <li key={product.id}
            onClick={self.handleProductClick.bind(self, product)}
            className={isExpanded || noneSelected ? '' : 'collapsed'}>
          {product.name} ({product.price})
          {details}
        </li>
      );
    });

    return (
      <ul>
        {products}
      </ul>
    );
  }
});

React.render(<ItemsList />, document.body);
{% endhighlight %}

My first reaction was "wow, more code, this can't be better". But it is. Why?

- data is nicely separated in `PRODUCTS` array
- all the state we have is stored in `expandedProductId`
- all presentation logic is written in `render` method
- we can read code top to bottom
- I could easily understand everything it does after quick review (you might
not know React yet, but it is fairly easy to learn)

## Conclusion

I know this is pretty trivial example. But it still shows the fundamental
differences between the libraries and how they help you separate concerns.
