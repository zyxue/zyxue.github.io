---
layout: post
title: Notes on python-json-logger
author: Zhuyi Xue
tags: python, logging
---

Code example for using [https://github.com/madzak/python-json-logger](https://github.com/madzak/python-json-logger).

{% highlight python %}
import logging
import time

from pythonjsonlogger import jsonlogger

logger = logging.getLogger()
logger.setLevel(logging.INFO)

logHandler = logging.StreamHandler()
logHandler.setLevel(logging.DEBUG)

formatter = jsonlogger.JsonFormatter(fmt="%(asctime)s|%(levelname)s|%(message)s")
formatter.converter = time.gmtime
formatter.default_time_format = "%Y-%m-%dT%H:%M:%S"
formatter.default_msec_format = "%s.%03dZ"

logHandler.setFormatter(formatter)
logger.addHandler(logHandler)

# Log only "message" and no other keys.
logger.info("message1")
# {"asctime": "2023-11-18T17:09:30.152Z", "levelname": "INFO", "message": "message1"}

# Log "message" and additional keys using `extra=`.
logger.info("message1", extra=dict(key1="123"))
# {"asctime": "2023-11-18T17:09:30.152Z", "levelname": "INFO", "message": "message1", "key1": "123"}

# Log "message" and additional keys using `dict`.
logger.info(dict(message="some message", key1="abc", key2="def"))
# {"asctime": "2023-11-18T17:09:30.152Z", "levelname": "INFO", "message": "some message", "key1": "abc", "key2": "def"}

# Log all keys with a dict without the `message` key. (Just curious what the output would look like, it feels unuseful in general)
logger.info({"abc": "def"})
# {"asctime": "2023-11-18T17:09:30.152Z", "levelname": "INFO", "message": "", "abc": "def"}

try:
    raise ValueError("abc")
except ValueError as err:
    # Log an exception.
    logger.error("some error", exc_info=True)
    # {"asctime": "2023-11-18T17:09:30.152Z", "levelname": "ERROR", "message": "some error", "exc_info": "Traceback (most recent call last):\n  File \"toy.py\", line 30, in <module>\n    raise ValueError(\"abc\")\nValueError: abc"}

    logger.error({"message": "some error", "key1": "abc"}, exc_info=True)
    # Log an exception with additional keys.
    # {"asctime": "2023-11-18T17:09:30.153Z", "levelname": "ERROR", "message": "some error", "key1": "abc", "exc_info": "Traceback (most recent call last):\n  File \"toy.py\", line 30, in <module>\n    raise ValueError(\"abc\")\nValueError: abc"}

{% endhighlight %}