---
layout: post
title:  Emacs cheat sheet
author: Zhuyi Xue
tags: emacs
---

It's easy to remember those key bindings that are used everyday
(e.g. navigation, search), but some ones, although handy, but may be
hard to recall if not used regularly. Hence, I put them here.


1. Using emacs to make a macro with increasing numbers (e.g. 0, 1, 2, 3, 4)
   ```
   F3 F3 <space> C+n C+a F4
   ```

1. Move point forward to the matching } bracket
   ```
   C-M-f
   ```

1. Insert the output of shell command into emacs buffer (very handy! Credit to
   [Anupam](https://stackoverflow.com/questions/12292239/insert-the-output-of-shell-command-into-emacs-buffer))
   ```
   C-u M-! <shell-command>
   ```


1. Inplace shell command 
   ```
   M-|
   ```


1. The following three are for getting help
   ```
   C-h f
   C-h s
   C-h S
   ```

1. Compilation (not remember I've ever used this one)
   ```
   emacs --batch --eval '(byte-recompile-directory "~/.emacs.d")'
   # or to recompile a single file as from a Makefile,
   emacs --batch --eval '(byte-compile-file "your-elisp-file.el")'
   ```

