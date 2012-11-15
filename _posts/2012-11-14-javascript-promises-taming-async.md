---
layout: post
title: "javascript promises (taming async)"
tags: javascript promises q.js
---

I recently discovered the need for a real promise library (jQuery Deferreds
needed a little extra).  I found a library called
[q.js](https://github.com/kriskowal/q) which I've spent the last few days
attempting to wrap my head around.  This all started culminating after
reading [this excellent post](https://gist.github.com/3889970) about
*Missing the Point of Promises*.

Indeed, I was missing the point.

After dealing with jQuery deferreds inside of more jQuery deferreds, I
began looking around for a different way.  I thought back to the above
post and decided to start looking into using it.

I started by reading the main tutorial and found myself confused about
why all these methods were necessary AND how they all worked together.
I happened to stumble upon a file in the repository called
[README.js](https://github.com/kriskowal/q/blob/master/design/README.js).
I suggest going through and reading that for anyone that:

* wonders why they would want/need promises
* is confused after reading the main tutorial for the library
* wants a good understanding of the source of the library

I noticed quite quickly that not all the methods are documented in the
documentation and that you will need to peek at the source to
find/understand some (which is unfortunate).

Here's a quick demo of something you can do that helped me make sense
of promises:

    Q.when($.get('/data/'))  // convert jQuery Deferred "thenable" into Q promise
    .then(function(data) {
      console.log('success', data);
      data.key = 'something else';
      return Q.when($.post('/data/', data)); // convert to Q promise
    })
    .then(function(postdata) {
      // do something with postdata
    })
    .fail(function() {
      // handles failure of all above jQuery Deferred's (get/post)
      console.log('failed', arguments);
    })
    .done();

Notice how the flow is linear down the page instead of nested inside of
lower scopes.  jQuery Deferreds get you pretty far, but it doesn't chain
like Q promises do which make things feel very linear instead of async
(which is the point of promises).

Another piece of magic above is the `.done()` method which can be
demonstrated by the following.

    Q.fcall(function() {
      var obj = {};
      if (obj.attr.oops) {
        // the above blows up because "attr" doesnt exist and "oops"
        // is not a method of "undefined"
      }
    })
    .done();

In this case, the uncaught Exception is caught inside of the promise
and reraised if not handled by fail handler if `.done()` is at the
end of the chain.  It means its very easy to construct something like
the following python coders would be familiar with:

    try:
        something()
    except Exception, e:
        handle_exception(e)
    finally:
        cleanup()

With q.js, it looks like this:

    Q.fcall(function() {
      throw new Error('boom');
    })
    .fail(function(e) {
      // e is Error('boom')
    })
    .fin(function() {
      // here is your cleanup() method
    })
    .done();

Python has it as part of the syntax, but promises make the same function
possible but also adding handling of async easily.

The q.js library also has a deferred similar in function to the jQuery
Deferred but with all the guarantees/features of Q.

    var defer = Q.defer();
    var promise = defer.promise; // principle of least authority

    // a promise here is exactly the same as what was accomplished above
    promise.then(function(value) {
      console.log(value);
    }, function(fail_value) {
      console.log(fail_value);
    })
    .done();

    // in this case, console.log(value) will print 10
    setTimeout(function() {
      defer.resolve(10);
    }, 5000); // wait 5 seconds, or any async method

    // OR

    // in this case, console.log(fail_value) will print Error('boom')
    setTimeout(function() {
      defer.reject(new Error('boom'));
    }, 5000);

Also with deferreds, progress bars become trivial:

    var defer = Q.defer();
    var promise = defer.promise;

    promise.progress(function(mesg) {
      console.log(mesg);
    })
    .then(function(value) {
      console.log(value);
    })
    .done();

    setTimeout(function() {
      defer.notify('25% complete');
    }, 1000);

    setTimeout(function() {
      defer.notify('50% complete');
    }, 2000);

    setTimeout(function() {
      defer.notify('75% complete');
    }, 3000);

    setTimeout(function() {
      defer.notify('100% complete');
      defer.resolve('done');
    }, 4000);

    // the above prints:
    // > 25% complete
    // > 50% complete
    // > 75% complete
    // > 100% complete
    // > done

There are plenty of other interesting and exciting ways to use the library.
The above got me started to the point where I could start exploring other
uses and I wouldn't get bogged down in details and overly confused.
