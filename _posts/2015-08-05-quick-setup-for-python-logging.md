---
layout: post
title:  Quick Setup for Python logging
author: Zhuyi Xue
tags: python,logging
---

* toc
{:toc}


## Quick snippet for setting up logging

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

## Update on 2023-12-12: in-depth understanding of logging

#### Default levels of loggers and handler

```
CRITICAL = 50
FATAL = CRITICAL
ERROR = 40
WARNING = 30
WARN = WARNING
INFO = 20
DEBUG = 10
NOTSET = 0
```
Ref: [logging/__init__.py#L98-L105](https://github.com/python/cpython/blob/c6e614fd81d7dca436fe640d63a307c7dc9f6f3b/Lib/logging/__init__.py#L98-L105).

Default root logger has level `WARNING`, default module logger level has
`NOSET`, default StreamHandler has level `NOSET` as shown below.

```
>>> import logging
>>> logging.getLogger().level # root logger level
30
>>> logging.getLogger('foo').level # module logger level
0
>>> logging.StreamHandler().level
0
```

#### Interaction of different levels

Pseduo code for how different levels interact:
```
if log.level > logger.effectiveLevel:
    if log.level > handler.level # default NOSET:
         emit(log)
    else:
         no emit.
```

Note `effectiveLevel` is the first *non-zero* level in the logger hierarchy
(see
[`getEffectiveLevel`](https://github.com/python/cpython/blob/c6e614fd81d7dca436fe640d63a307c7dc9f6f3b/Lib/logging/__init__.py#L1744-L1749)).
A caveat is when the levels of both the root logg and a module logger are kept
as default (`WARNING` and `NOSET` (0), respectively), then the effective level
of the module logger would be `WARNING`, inherited from root logger, which
might lead to confusing behavior, e.g. this code won't log anything

```
mod = logging.getLogger("foo")
print(f"{mod.level=:}")
print(f"{mod.getEffectiveLevel()=:}")
mod.addHandler(logging.StreamHandler())
mod.info("abc")
```

The output shows that the mod logger has an effective level of 30, which is
inherited from root logger given itself doesn't have a level.

```
mod.level=0
mod.getEffectiveLevel()=30
```

But if you set the mod logger level to 1, it'll emit the log

```
mod = logging.getLogger("foo")
mod.setLevel(1)
print(f"{mod.level=:}")
print(f"{mod.getEffectiveLevel()=:}")
mod.addHandler(logging.StreamHandler())
mod.info("abc")
```
the output is 
```
mod.level=1
mod.getEffectiveLevel()=1
abc
```

#### Reason to duplicated logs

Sometimes one may see one log gets duplicated twice, a common reason could be
that both the module logger and root logger has a Streamhandler configured, so
logs from the module logger will get emitted twice, e.g.

```
root = logging.getLogger()
mod = logging.getLogger("foo")

handler = logging.StreamHandler()
handler.setFormatter(
    logging.Formatter(fmt="%(asctime)s|%(name)s|%(levelname)s|%(message)s")
)

root.addHandler(handler)
mod.addHandler(handler)

mod.warning("abc")
```
The output is
```
2023-12-12 17:18:05,566|foo|WARNING|abc
2023-12-12 17:18:05,566|foo|WARNING|abc
```

#### Simple logging strategy

One simple logging strategy is to reserve root logger for 3rd-party library
loggers and set propagate of 1st-party modules logger to False to avoid log
duplication, e.g.

{% gist fcbc9ddece7e0abf76c56cb49ed76bb0 %}

Then, with the following setup:

```
# first_party.py
import log_utils

logger = log_utils.get_logger(__name__)


def log_something():
    logger.info("I'm first party.")
    logger.warning("I'm first party sending a warning.")


# third_party.py
import logging

logger = logging.getLogger(__name__)


def log_something():
    logger.debug("I'm third party debugging.")
    logger.warning("I'm third party sending a warning.")

# main.py
import log_utils
import first_party
import third_party

log_utils.configure_root_logger()

first_party.log_something()
third_party.log_something()
```

Running `main.py` outputs
```
2023-12-12 22:12:39,756|first_party|INFO|I'm first party.
2023-12-12 22:12:39,756|first_party|WARNING|I'm first party sending a warning.
2023-12-12 22:12:39,757|third_party|WARNING|I'm third party sending a warning.
```

Note the debug message from `third_party.py` does not get emitted as expected.


## Update on 2018-10-10: quick `setup_log` (deprecated)

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

## Update on 2017-06-07: notes from Stack Overflow

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

## Update on 2016-04-13: [colorlog]((https://github.com/borntyping/python-colorlog)

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
