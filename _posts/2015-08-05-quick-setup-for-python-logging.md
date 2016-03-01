---
layout: post
title:  Quick Setup for Python logging
author: Zhuyi Xue
tags: shell,bash,navigation
---

The following snippet serves as a cheat sheet for quickly setting up Python
module in more detailed output format.

{% highlight python %}
import logging  
logging.basicConfig(
    level=logging.DEBUG, format='%(asctime)s|%(levelname)s|%(message)s')
{% endhighlight %}


For complete setup instruction, see Python logging module
[documentation](https://docs.python.org/2/library/logging.html), please.
