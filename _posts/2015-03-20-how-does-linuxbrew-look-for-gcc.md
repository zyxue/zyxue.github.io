---
layout: post
title:  How does linuxbrew looks for gcc
author: Zhuyi Xue
tags: linuxbrew,gcc
---

For example, when looking for `gcc-4.4`, use `--cc=gcc-4.4` or set
`HOMEBREW_GCC=gcc-4.4` and `gcc-4.4` must be in environmental variable `PATH`,
and its name CANNOT be `gcc44` or anything else. You could do something like
this

{% highlight bash %}
ln -s /usr/bin/gcc44 $prefix/bin/gcc-4.4
ln -s /usr/bin/g++44 $prefix/bin/g++-4.4
ln -s /usr/bin/gfortran44 $prefix/bin/gfortran-4.4
{% endhighlight %}
