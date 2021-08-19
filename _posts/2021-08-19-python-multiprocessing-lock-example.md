---
layout: post
title: Python multiprocessing lock example
author: Zhuyi Xue
tags: operating system, process
---

This post uses A simple example to illustrate the effect of a
[`multiprocessing.Lock`](https://docs.python.org/3/library/multiprocessing.html#multiprocessing.Lock)
in Python. 

We will start multiple processes, each of which print the string `hello world`
followed by a process index. The process index just shows the order in which
the processes are started. By turning on and off the lock, we see how it
affects the printed output.


Here is the code:

<script src="https://gist.github.com/zyxue/77232cb720c641bcd9cf7f81ae06f1b1.js"></script>

If we uncomment the Line 37 that does not use a lock, the output will be scrambled like

```
hhhheelellelllollooo   wo wowwrorolrrllddld  1
  2
d  3
  0
```

If we uncomment the Line 38 that uses a lock, the output will be like

```
hello world  0
hello world  2
hello world  3
hello world  1
```

