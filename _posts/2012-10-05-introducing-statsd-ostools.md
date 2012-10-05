---
layout: post
title: "introducing statsd-ostools"
tags: django django-statsd statsd statsd-ostools
---

I recently deployed [statsd](https://github.com/etsy/statsd) and
[graphite](http://graphite.wikidot.com/) after reading through etsy's blog post
[measure anything, measure everything](http://codeascraft.etsy.com/2011/02/15/measure-anything-measure-everything/).
Finally, I found [django-statsd](https://github.com/andymckay/django-statsd)
(pypi package is actually *django-statsd-mozilla*) for some quick and easy
integration with Django.  However, I was unable to find a way to easily
monitor common unix OS information like `iostat`, `mpstat` and `vmstat` for
instance.

So I set out to write something simple to do it and created **statsd-ostools**
as a result.  At the moment, it really only supports linux's `iostat`,
`mpstat`, and `vmstat`, but I plan to add support for other platforms in the
future.

So take a look at [statsd-ostools](https://github.com/dlamotte/statsd-ostools)
and let me know what you think.  And/or install it via `pip` or `easy_install`:

    easy_install http://pypi.python.org/pypi/statsd-ostools
    pip install http://pypi.python.org/pypi/statsd-ostools

statsd-ostools is written in python and only depends on `setproctitle` and
`statsd` so use it on any of your linux servers to monitor them.
