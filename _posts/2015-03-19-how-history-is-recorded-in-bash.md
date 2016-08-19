---
layout: post
title:  How history is recorded in Bash
author: Zhuyi Xue
tags: bash,history
---

1. When a new shell starts, it reads the `~/.bash_history` file.

1. As new commands are typed, typing `history` will still only show the new
   commands, but they're only stored in the memory temporarily and not yet
   appended to `~/.bash_history`. Therefore, if you open a new bash shell while
   keeping the current shell running, typing `history` in the new shell won't
   output the history of the recently typed commands in the current shell.

1. If you close the current shell, the newly typed commands will be written to
   the end of `~/.bash_history`. Therefore, If you open a new terminal after the
   current one is closed, `history` in the new shell will output the most recent
   commands typed in the closed shell. 

The above description is based on my own experiment, feel free to correct me if
I am wrong or incomplete in any sense.
