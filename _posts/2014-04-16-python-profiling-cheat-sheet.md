---
layout: post
author: Zhuyi Xue
tags: python,profiling
---

### On the commandline:

{% highlight bash %}
python -m cProfile -s cumtime myscript.py
{% endhighlight %}

where the argument to `-s` can be many other things:

* `calls`: call count
* `cumulative`: cumulative time
* `cumtime`: cumulative time
* `file`: file name
* `filename`: file name
* `module`: file name
* `ncalls`: call count
* `pcalls`: primitive call count
* `line`: line number
* `name`: function name
* `nfl`: name/file/line
* `stdname`: standard name
* `time`: internal time
* `tottime`: internal time


For more details, see
[https://docs.python.org/2/library/profile.html#pstats.Stats.sort_stats](https://docs.python.org/2/library/profile.html#pstats.Stats.sort_stats)


### In IPython:

You could time a single run with `%time`, or calculate average time spent over
multiple runs with `%timeit`, and `prun` for profiling. e.g.

`%time`

<pre>
In [13]: %time 1 + 1
CPU times: user 4 µs, sys: 1 µs, total: 5 µs
Wall time: 6.91 µs
Out[13]: 2
</pre>

`%timeit`

<pre>
In [14]: %timeit 1 + 1
100000000 loops, best of 3: 14.2 ns per loop
</pre>

`%prun`

{% highlight default %}
In [17]: %prun for i in range(100000): 1+1
         3 function calls in 0.005 seconds

   Ordered by: internal time

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.005    0.005    0.005    0.005 <string>:1(<module>)
        1    0.000    0.000    0.005    0.005 {built-in method builtins.exec}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}
{% endhighlight %}


There are also

* `%lprun`, which can profile each line of a script, and
* `%mpruun` which can profile memory usage of a script

But they need external modules
[`line-profiler`](https://github.com/rkern/line_profiler) and
[`memory_profiler`](https://pypi.python.org/pypi/memory_profiler) installed. I
haven't used the two extensively, but it may be worthwhile to checkout. See this
[post](http://pynash.org/2013/03/06/timing-and-profiling/) for some usage
examples.
