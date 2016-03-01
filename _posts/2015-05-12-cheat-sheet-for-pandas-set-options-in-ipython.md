---
layout: post
title:  Cheat sheet for pandas.set_options in ipython
author: Zhuyi Xue
tags: cheat sheet,python,pandas,ipython
---

Sometimes it can be annoying that the default display.width (aka line_width,
but deprecated) is too narrow to display the entire dataframe clearly when
using [pandas](http://pandas.pydata.org/) in [iPython](http://ipython.org/), so
do this in iPython:

{% highlight python %}
import pandas as pd
pd.set_option('display.width', 200)
{% endhighlight %}

If each column is narrow, but you have quite a number of columns and would like
to show them on one line, then do this:

{% highlight python %}
pd.set_option('max_columns', 20)
{% endhighlight %}

If you don't have many columns, and want each column to a bigger width,

{% highlight python %}
pd.set_option('display.max_colwidth', 300)
{% endhighlight %}
