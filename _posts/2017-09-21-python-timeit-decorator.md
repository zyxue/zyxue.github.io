---
layout: post
title: Python timeit decorator
author: Zhuyi Xue
tags: python, time, timeit, decorator
---

**Update 2023-12-02:**

A more properly typed version:

{% highlight python %}
import datetime
import time
from functools import wraps
from typing import Callable, TypeVar

from typing_extensions import ParamSpec

P = ParamSpec("P")
T = TypeVar("T")


def timeit(func: Callable[P, T]) -> Callable[P, T]:
    """Times a function, usually used as decorator"""

    @wraps(func)
    def timed_func(*args: P.args, **kwargs: P.kwargs) -> T:
        """Returns the timed function"""
        start_time = time.time()
        result = func(*args, **kwargs)
        elapsed_time = datetime.timedelta(seconds=(time.time() - start_time))
        print("time spent on %s: %s", func.__name__, elapsed_time)
        return result

    return timed_func
{% endhighlight %}

**Update 2020-04-13:**

A simpler version:

{% highlight python %}
import datetime
import time
from functools import wraps
from typing import Any, Callable


def timeit(func: Callable[..., Any]) -> Callable[..., Any]:
    """Times a function, usually used as decorator"""

    @wraps(func)
    def timed_func(*args: Any, **kwargs: Any) -> Any:
        """Returns the timed function"""
        start_time = time.time()
        result = func(*args, **kwargs)
        elapsed_time = datetime.timedelta(seconds=(time.time() - start_time))
        print("time spent on %s: %s", func.__name__, elapsed_time)
        return result

    return timed_func
{% endhighlight %}

**Before 2020-04-13:**

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
