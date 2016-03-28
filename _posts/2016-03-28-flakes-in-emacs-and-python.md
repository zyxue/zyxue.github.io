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

* [autoflake](autoflake) uses [pyflakes](pyflakes) (faster than [pylint](pyflint) or [pychecker](pychecker))
* [flake8](flake8) is a wrapper around [pyflakes](pyflakes), [pep8](pep8), and McCabe script

[autoflake]: https://github.com/myint/autoflake
[pyflakes]: https://pypi.python.org/pypi/pyflakes
[pylint]: https://www.pylint.org/
[pychecker]: http://pychecker.sourceforge.net/
[flake8]: https://pypi.python.org/pypi/flake8
[pep8]: https://pypi.python.org/pypi/pep8

## Emacs

* [Emacs flycheck](https://github.com/flycheck/flycheck) intends to be a replacement for [Emacs flymake](flymake)
* [flymake-python-pyflakes.el](https://github.com/purcell/flymake-python-pyflakes) uses [Emacs flymake](flymake) + [pyflakes](pyflakes) or [flake8](flake8)


[flymake]: https://www.emacswiki.org/emacs/FlyMake
