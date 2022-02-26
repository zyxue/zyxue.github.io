---
layout: post
title:  Convert Python profile output to Pandas DataFrame
author: Zhuyi Xue
tags: python cProfile
---

Just an example for converting Python profile output to a [Pandas
DataFrame](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)
for slicing and dicing.

Take this python code as an example. Suppose it's saved in `toy.py`

{% highlight python %}
def f():
    s = 0
    for i in range(10000):
        s += i
    return s

f()
{% endhighlight %}

Then run `python -m cProfile toy.py | tee profile.log`.

```
         4 function calls in 0.001 seconds

   Ordered by: standard name

   ncalls  tottime  percall  cumtime  percall filename:lineno(function)
        1    0.000    0.000    0.001    0.001 toy.py:1(<module>)
        1    0.001    0.001    0.001    0.001 toy.py:1(f)
        1    0.000    0.000    0.001    0.001 {built-in method builtins.exec}
        1    0.000    0.000    0.000    0.000 {method 'disable' of '_lsprof.Profiler' objects}

```

The log can be converted to a Pandas DataFrame with this code snippet:

{% highlight python %}
import pandas as pd


def profile2df(profile_path: str) -> pd.DataFrame:
    """Converts log from Python cProfile to a Pandas DataFrame."""

    outs = []
    num_lines_to_skip = 4
    num_columns = 6
    with open(profile_path, "rt") as opened:
        for index, line in enumerate(opened):
            if index < num_lines_to_skip:
                continue

            if index == num_lines_to_skip:
                columns = line.split()
                continue

            # ignore empty lines
            if not line.strip():
                continue

            outs.append(line.split(maxsplit=num_columns - 1))

    return pd.DataFrame(outs, columns=columns)


if __name__ == "__main__":
    df = profile2df("./profile.log")
    print(f"{df=:}")
{% endhighlight %}

Run the above code, and the output is like

```
df=  ncalls tottime percall cumtime percall                          filename:lineno(function)
0      1   0.000   0.000   0.001   0.001                               toy.py:1(<module>)\n
1      1   0.001   0.001   0.001   0.001                                      toy.py:1(f)\n
2      1   0.000   0.000   0.001   0.001                  {built-in method builtins.exec}\n
3      1   0.000   0.000   0.000   0.000  {method 'disable' of '_lsprof.Profiler' object...
```



### References

* See more info about cProfile at [https://docs.python.org/3/library/profile.html](https://docs.python.org/3/library/profile.html).
