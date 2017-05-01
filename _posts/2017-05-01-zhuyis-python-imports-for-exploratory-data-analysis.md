---
layout: post
title: Zhuyi's Python imports for exploratory data analysis
author: Zhuyi Xue
tags: Python, data analysis, Jupyter
---

With Python [Jupyter](http://jupyter.org/) and
[SciPy stack](https://www.scipy.org/stackspec.html), we can quickly set up a
very interactive environment for exploratory data analysis (EDA).

Here I listed the libraries I normally used. I just import all of them in the
first cell in a Jupyter notebook, and then start working on the data. When the
analysis is almost done, then I will start cleaning up and removing the
redundant ones. I find easier to remove lines than to think of the exact name of
a library/module to import.

The list could be long, and for clarity, I usually loosely categorize them into
a few groups groups separated by empty newline such as:

1. Python standard libraries
1. Scipy Stack libraries
1. Other third-party libraries
1. My own written modules

The imports are loosely ordered by usage frequency or name length.

At the end of imports, there are also commonly used setup for libraries such as
[matplotlib](https://matplotlib.org/) and [pandas](http://pandas.pydata.org/),
and
[IPython magic commands](https://ipython.org/ipython-doc/3/interactive/magics.html).


{% gist zyxue/ea633c70f6bca2c12ffbc4de792419a2 %}
