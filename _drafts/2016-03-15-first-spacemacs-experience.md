---
layout: post
title:  First experience with Spacemacs
author: Zhuyi Xue
tags: spaceemacs,emacs,python
---

I am a 5-year heavy user, and I write almost everything in emacs. I have been
hearing [Spacemacs](https://github.com/syl20bnr/spacemacs) several times before
I actually try it recently. Here I summarize my first hands on experience with
it. Also, since I am a Python developer, I will also comment a little on Python
development environment setup at the end.


Overall, I am really impressed by the outlook of spacemacs. I have my own
configuration accumulated in the past 5 years, and I have enjoyed using
[Solarized](http://ethanschoonover.com/solarized) color theme for a long time.
But when it compares to spacemacs, it still looks so out of date, like in the
80s. This is the first reason that gets me attracted to spaceemacs. The other
reason is its layers system. I currently think it as a way to help modularize
all the emacs configuration, which I have been doing manually myself, and I
version controlled all my configurations to a git repo. Since spaceemacs is a
community based project, I tend to trust its structure would be much more
elegant, maintainable and scalable. I still need to learn more about the layers
system though.

Install

    git clone https://github.com/syl20bnr/spacemacs ~/.emacs.d
    
Open emacs, it will ask for what mode you'd like to choose, `vim` or `emacs`.
Spaceemacs was developed for `vim` users, but now it doesn't matter any more. I
chose the latter.

In the `~/spacemacs`, uncommented

* `auto-completion`
* `syntax-checking`

and added 

* `python`

for first trial of developing in a python file.

Then find a python script, which could be in an existing project. And open it,
type `M-m e l` to show all existing error detected by
[`flak8`](https://pypi.python.org/pypi/flake8).

Need a screenshot to show example of listed errors.
