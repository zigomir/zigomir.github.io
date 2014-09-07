---
layout: post
title: "Testing JavaScript with Karma test runner"
date: 2013-10-28 21:04
published: false
categories: javascript testing
---

Even jQuery spaghetti can be tested to some extent.

<!-- more -->

# DON'T PUBLISH

Because in this test I only test jQuery's `.text()` method and click event handler. This is not our domain logic so we are not testing something that will make our life easier.

### User story:

As an user I want to see new header text when clicking on it. (I know, its trivial and boring example, but I want to make it testable)

### Prerequisites

Node.js & npm

For this example I'll use [Karma runner](http://karma-runner.github.io/0.10/index.html) because it’s popular and you can use it in your Continuous Integration process, since it is running from  your terminal.

### Installation

{% highlight bash %}
npm install -g karma
karma start
{% endhighlight %}

### Configuration file
This is an example. Put this into a root of your project and name it `karma.conf.js`.

{% highlight javascript %}
module.exports = function(config) {
  config.set({
    // base path, that will be used to resolve files
    // if you set it to current dir like here, all your other paths can just be relative to it
    basePath: ".",
    frameworks: ["jasmine"],
    // list of files / patterns to load in the browser
    files: [
      // serve html fixtures
      { pattern: "test/fixtures/*.html", watched: true, served: true, included: false },
      // dependencies
      "app/lib/jquery.min.js",
      // test helper code
      "test/helpers/jasmine-jquery.js",
      // set jasmine fixtures path
      // includes only this line: jasmine.getFixtures().fixturesPath = "base/test/fixtures/";
      "test/helpers/fixtures.js",
      // code you want to test
      "app/app.js",
      // test code
      "test/spec/*.js"
    ],

    // list of files to exclude
    exclude: [],
    preprocessors: {"*/.html": [] },

    // test results reporter to use
    // possible values: "dots", "progress", "junit"
    reporters: ["progress"],

    // web server port
    port: 9876,

    // enable / disable colors in the output (reporters and logs)
    colors: true,

    // level of logging
    // possible values: LOG_DISABLE || LOG_ERROR || LOG_WARN || LOG_INFO || LOG_DEBUG
    logLevel: config.LOG_INFO,

    // enable / disable watching file and executing tests whenever any file changes
    autoWatch: true,

    // Start these browsers, currently available:
    // - Chrome
    // - ChromeCanary
    // - Firefox
    // - Opera
    // - Safari (only Mac)
    // - PhantomJS
    // - IE (only Windows)
    browsers: ["PhantomJS"],

    // If browser does not capture in given timeout [ms], kill it
    captureTimeout: 20000,

    // Continuous Integration mode
    // if true, it capture browsers, run tests and exit
    singleRun: false,

    plugins: [
      "karma-jasmine",
      "karma-phantomjs-launcher"
    ]
  });
};
{% endhighlight %}

### HTML fixtures

These will hold the elements you want to test any DOM operations on.
Put them into `/test/fixtures/*.html` files.

We are only interested in `h1` element

*Example file*

{% highlight html %}
<h1>I can see in your machine</h1>
{% endhighlight %}

### Test

The code to test your application code is written for [Jasmine testing framework](http://pivotal.github.io/jasmine/)

{% highlight javascript %}
describe("D0m1n470r", function() {
  it("will change header text", function() {
    var $header = $("h1");
    expect($header.text()).toBe("I can see in your machine");
    clickCallback("h1", "No");
    expect($header.text()).toBe("No");
  });
});
{% endhighlight %}

### Implementation

Now we can write implementation code for clickCallback

{% highlight javascript %}
var clickCallback = function(selector, text) {
  $(selector).text(text);
};
{% endhighlight %}

### JSbin
<a class="jsbin-embed" href="http://jsbin.com/uRiCACU/1/embed?html,js,output">JS Bin</a>
<script src="http://static.jsbin.com/js/embed.js"></script>

### Conclusion

This is a lot of work to test this trivial user story. But you know that code will quickly become more complicated.
Writing testable code also have advantages to force you write testable functions. In our example you would probably
write an anonymous function for the click callback. This is all good for so simple functions.
But when you need to implement more logic I prefer to extract the code into smaller functions instead.
