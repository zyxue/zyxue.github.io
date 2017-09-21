---
layout: post
title: Python timeit decorator
author: Zhuyi Xue
tags: python, time, timeit, decorator
---

A plain python `timeit` decorator, time any function

{% highlight python %}
import time
from functools import update_wrapper


def decorator(d):
    "Make function d a decorator: d wraps a function fn."
    def _d(fn):
        return update_wrapper(d(fn), fn)
    update_wrapper(_d, d)
    return _d


@decorator
def timeit(f):
    """time a function, used as decorator"""
    def new_f(*args, **kwargs):
        bt = time.time()
        r = f(*args, **kwargs)
        et = time.time()
        print("time spent on {0}: {1:.2f}s".format(f.__name__, et - bt))
        return r
    return new_f
{% endhighlight %}


### Example usage:


{% highlight python %}
# As a decorator:
@timeit
def your_awesome_function(msg):
    time.sleep(0.1)             # just for illustration purpose
    print(msg)
	
your_awesome_function('timeit used as a decorator')


# As a function
def another_awesome_function(msg):
    time.sleep(0.15)            # just for illustration purpose
    print(msg)

# as a function, timeit returns a decorated function
timeit(another_awesome_function)('timeit used as a function')
{% endhighlight %}
