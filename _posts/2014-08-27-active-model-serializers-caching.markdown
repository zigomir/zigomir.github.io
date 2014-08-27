---
layout: post
title: "Cache active model serializers version >= 0.9.0"
date: 2014-08-27 07:56
comments: true
categories: active_model_serializers rails caching
---

Since caching [is not implemented yet](https://groups.google.com/forum/#!topic/rails-api-core/FRy7GMbGEMI)
in new version of AMS (0.9.0) because of rewrite you can use Rails built in caching functionality. 

{% highlight ruby %}
def index
    trips = Trip.all
    
    json = cache ['v1', trips] do
      render_to_string json: trips
    end
    
    render json: json
end
{% endhighlight %}

It won't work in development by default because caching is disabled for development mode.

You can copy config line from your `production.rb` to `development.rb` just to test if it's working:

{% highlight ruby %}
config.action_controller.perform_caching = true
{% endhighlight %}
