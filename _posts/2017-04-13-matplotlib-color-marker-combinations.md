---
layout: post
<!-- title: Javascript prototype exploration -->
author: Zhuyi Xue
tags: Python
---

Once in a while, you want to draw so many lines in one single plot, and you run
out of default color options. Then, one way to rescue is to add different
markers. 

I use the following combinations to draw many lines in a single plot in Python
with [matplotlib](https://matplotlib.org/). The same idea applies to other
plotting libraries in other languages, too.

{% highlight python %}
import itertools

# https://matplotlib.org/api/markers_api.html
markers = [None, '.', ',', 'o', 'v', '^', '<', '>', '1', '2', '3', '4', '8', 's', 'p', 'P', '*', 'h', 'H', '+', 'x', 'X', 'D']
# http://matplotlib.org/api/colors_api.html
colors = ['blue', 'green', 'red', 'cyan', 'magenta', 'yellow', 'black']

mc_list = list(itertools.product(markers, colors))
Output:
{% endhighlight %}

You could remove certain markers to colors accordingly based on your own
preferences. The following subset works well for me.

{% highlight python %}
import itertools

markers = ['o', 'v', '^', '<', '>', '1', '2', '3', '4', '8', 's', 'p', 'P', '*', 'h', 'H', '+', 'x', 'X', 'D']
colors = ['blue', 'green', 'red', 'magenta']

mc_list = list(itertools.product(markers, colors))
Output:
{% endhighlight %}

Here is an example:

{% highlight python %}
import matplotlib.pyplot as plt
import numpy as np

markers = ['o', 'v', '^', '<', '>', '1', '2', '3', '4', '8', 's', 'p', 'P', '*', 'h', 'H', '+', 'x', 'X', 'D']
colors = ['blue', 'green', 'red', 'magenta']
mc_list = list(itertools.product(markers, colors))

num_lines = 12
xs = np.arange(0, 10, 1)
for k, i in enumerate(np.arange(num_lines, 0, -1)):
    ys = xs * i
    mkr, col = mc_list[k]
    plt.plot(xs, ys, marker=mkr, color=col, label=k)

plt.xlim(-1, 10)
plt.ylim(-10, 130)
plt.legend(loc='upper left', bbox_to_anchor=(1.02, 1.03), numpoints=1)

{% endhighlight %}

<p><img src="/assets/many-lines-plot-example.png" alt="many-lines-plot-example.png" width="600"/></p>

I think I would never use up all the combinations as the plot would become too
busy at some point.

Have fun. :)
