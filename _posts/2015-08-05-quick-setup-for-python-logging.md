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

#### Update on 2018-10-10

Quick setup for logging to both a file and the console

{% highlight python %}
def setup_log(log_file):
    log_format = '%(asctime)s|%(levelname)s|%(message)s'

    logging.basicConfig(
        level=logging.DEBUG,
        format=log_format,
        filename=log_file,
        filemode='a'
    )

    console_handler = logging.StreamHandler()
    console_handler.setLevel(logging.DEBUG)
    console_handler.setFormatter(logging.Formatter(log_format))

    # without name, getLogger returns the root logger
    root_logger = logging.getLogger()
    root_logger.addHandler(console)
{% endhighlight %}

#### Update on 2017-06-07:

Relationships among logger, handler, and formatter:

1. different loggers are responsible of logging status of different parts of the
   code (e.g. a library, module)
1. logger uses handler to writing logging message to a output media (e.g screen,
   file)
1. handler uses formatter to format the logging message
1. handler's logging level overwrites a logger's level. 


> Logger logging level as a global restriction on which messages are
> "interesting" for a given logger and its handlers. The messages that are
> considered by the logger afterwards get sent to the handlers, which perform
> their own filtering and logging process.

Quoted from
[https://stackoverflow.com/questions/17668633/what-is-the-point-of-setlevel-in-a-python-logging-handler](https://stackoverflow.com/questions/17668633/what-is-the-point-of-setlevel-in-a-python-logging-handler).

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
