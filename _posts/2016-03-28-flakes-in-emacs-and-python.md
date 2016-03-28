---
layout: post
title:  About flakes in Python and Emacs
author: Zhuyi Xue
tags: emacs,python,flake
---

Here I briefly summarize a few options available for flaking in Python and
Emacs. When they're used together, it becomes really handy to check for errors
while editing (i.e. at editing time).

## Python

* [autoflake](https://github.com/myint/autoflake) uses
  [pyflakes](https://pypi.python.org/pypi/pyflakes) (faster than
  [pylint](https://www.pylint.org/) or
  [pychecker](http://pychecker.sourceforge.net/))
* [flake8](https://pypi.python.org/pypi/flake8) is a wrapper around
  [pyflakes](https://pypi.python.org/pypi/pyflakes),
  [pep8](https://pypi.python.org/pypi/pep8), and McCabe script

## Emacs

* [Emacs flycheck](https://github.com/flycheck/flycheck) intends to be a
  replacement for [Emacs flymake](https://www.emacswiki.org/emacs/FlyMake)
* [flymake-python-pyflakes.el](https://github.com/purcell/flymake-python-pyflakes)
  uses [Emacs flymake](https://www.emacswiki.org/emacs/FlyMake) +
  [pyflakes](https://pypi.python.org/pypi/pyflakes) or
  [flake8](https://pypi.python.org/pypi/pep8)

