---
layout: post
title: Python SciPy Stack
author: Zhuyi Xue
tags: Python
---

If you are doing data analysis-related work and your primary language is Python,
I would highly recommend to get familiar with the Python SciPy Stack, which will
help you boost your productivity significantly. 

According to the [home page](https://www.scipy.org/stackspec.html), if a Python
distribution would like to promote itself as providing the SciPy Stack, it
should include following 8 required components

1. [Python](https://www.python.org/), the implementation of the Python language
   itself. For example, the most popular implementation is
   [CPython](https://en.wikipedia.org/wiki/CPython) (Not to be confused with
   [Cython](http://cython.org/), they are very different things). Other less
   popular ones include [IronPython](http://ironpython.net/),
   [Jython](http://www.jython.org/), [PyPy](http://pypy.org/).
1. [NumPy](http://www.numpy.org/), the Python package for numerical computation.
1. [SciPy](http://www.scipy.org/), a wide collection of numerical algorithms in
   e.g. optimization and statistics. It's noteworthy that SciPy is so fast
   because it depends on other libraries such as
   [BLAS (Basic Linear Algebra Subprograms)](http://www.netlib.org/blas/) and
   [LAPACK (Linear Algebra PACKage)](http://www.netlib.org/lapack/), which are
   written in lower-level languages like FORTRAN and C. SciPy is also what the
   stack is named after.
1. [Matplotlib](http://matplotlib.org/), the de facto standard library for 2D
   plotting in Python.
1. [Pandas](http://pandas.pydata.org/), a library providing high-performance,
    easy-to-use data structures like DataFrame and
    Series, and data analysis tools for Python.
1. [SymPy](http://www.sympy.org/), a library for symbolic mathematics. It aims
   to become a full-featured computer algebra system (CAS) entirely written in
   Python.
1. [IPython](https://ipython.org/), a rich interactive interface, and
   significantly more powerful replacement to the default interpreter interface
   provided by CPython.
1. [nose](https://nose.readthedocs.io/), a framework for testing Python code.


In addition, some other Python libraries I have used and felt worth mentioning
are

* [scikit-image](http://scikit-image.org/) for image processing &
  [scikit-learn](http://scikit-learn.org/stable/) for machine learning, both of
  which are scikits (short for SciPy Toolkits), i.e. add-on packages for SciPy.
  See [here](http://scikits.appspot.com/scikits) for a comprehensive list of
  scikits.
* [pytest](http://doc.pytest.org/en/latest/), another Python testing framework
  which I find more friendly to use than the aforementioned nose. This is a very
  [good presentation](http://mathieu.agopian.info/presentations/2015_06_djangocon_europe/)
  from Mozilla talking about their experience of switching from nose to pytest.
* [Jupyter Notebook](http://jupyter.org/), originally named IPython notebook, it
  provides a web interface for interactive programming in multiple languages.

Getting familiar with all the above packages will give you an highly interactive
and productive experience when programming in Python.

To install the SciPy stack, [Anaconda](https://www.continuum.io/downloads) is
recommended. For other options, see the
[installation guide](https://www.scipy.org/install.html).
