---
layout: post
title:  Quick Setup for Python logging
author: Zhuyi Xue
tags: python,logging
---

The following snippet serves as a cheat sheet for quickly setting up Python
logging module with more detailed output format.

{% highlight python %}
import logging  
logging.basicConfig(
    level=logging.DEBUG, format='%(asctime)s|%(levelname)s|%(message)s')

logging.info('some message')
{% endhighlight %}

For complete setup instruction, please see the
 [documentation](https://docs.python.org/2/library/logging.html) for Python
 logging module.

#### Update on 2016-04-13:

Besides, if you have [colorlog](https://github.com/borntyping/python-colorlog)
installed, colored messages are better for visualization. A quick starting
configuration can be

{% highlight python %}
import logging
import colorlog

logger = colorlog.getLogger()
logger.setLevel(logging.DEBUG)
handler = logging.StreamHandler()
handler.setLevel(logging.DEBUG)
handler.setFormatter(
    colorlog.ColoredFormatter('%(log_color)s%(levelname)s:%(name)s:%(message)s'))
	logger.addHandler(handler)

# Basic setup ENDS here, below are just sample code for printing out some
# example messages of different levels.

logger.debug('some debug message')
logger.info('some info message')
logger.warning('some warning message')
logger.error('some error message')
logger.critical('some critical message')

# Also try a exception message
class ExampleException(Exception):
    pass
	
try:
    raise ExampleException('This is just an example exception')
except ExampleException as err:
	logger.exception(err)

{% endhighlight %}

Example of the above messages printed:

<!-- surprised that html can be included in markdown directly,
http://daringfireball.net/projects/markdown/syntax#html -->
<!-- also, too lazy to format the colors in raw html -->
<p><img src="/assets/colorlog_example_messages.png" alt="Colorlog example messages"
width="480"/></p>

For more information about colorlog, please see
[here](https://github.com/borntyping/python-colorlog).
