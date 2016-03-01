---
layout: post
title:  Effective shell navigations & operations in bash
author: Zhuyi Xue
tags: shell,bash,navigation
---

I see many of my colleagues still using arrow keys (`↑`, `↓`, `←`, `→`), to
navigate and `Ctrl + C/D` to do copy/paste in the
[bash](https://www.gnu.org/software/bash/) shell environment, which is
slow. In fact, you could boost your productivity immediately in a few minitues
by mastering the following key bindings. I summarized them from many years of
experience, and try to show you the most handy ones.

*Note*: In the Mac OS X Terminal.app, the alt key doesn't work by default. You
 need to open the terminal setting (`Command-,`), and in the Keyboard tab, tick
 `Use Option as Meta key`, and then you're good to go.

### Navigation:

Navigation by line, word or letter offers different levels of granularity.

* `Ctrl-a`: Go to the beginning of line, replacing `Home` key. Think a as in **a**head.
* `Ctrl-e`: Go to the end of line, replacing `End` key. Think e as in **e**nd.
	
* `Alt-f`: Skip one *word* forward. Think f as in **f**orward.
* `Alt-b`: Skip one *word* backward. Think b as in **b**ackward.

* `Ctrl-f`: Skip one letter forward, replacing `→` key.
* `Ctrl-b`: Skip one letter backward, replacing `←` key.

### <a name="del"/>Deletion:

Deletion by line, word or letter also offers different levels of granularity.
	
* `Ctrl-u`: delete to the beginning of line
* `Ctrl-k`: delete to the end of line
	
* `Alt-d`: delete to the next word
* `Alt-Delete`: delete to the previous word

* `Ctrl-d`: delete to the next letter
* `Delete`: delete to the previous letter

### Cut/Paste:

* `Ctrl-u`, `Ctrl-k`, `Alt-d` and `Alt-Delete` as introduced in the [Delete](#del)
  section above not only delete the target content, but also store the deleted
  content in memory for later use, so they're more like a typical Cut
  operation.
* `Ctrl-y`: paste the cutted content. Think y as in **y**ank. Yank is related to
  paste or copy for some reason I am not sure of. If you happened to know,
  please drop me a message.
* `Alt-y`: paste the previously cutted content in reverse order.

I don't know any straightforward copy operation in the shell environment, but
it could be easily accomplished with 1 cut and 2 paste
operations. e.g. `Ctrl-d`-`Ctrl-y`-`Ctrl-y`.

### History commands management:

* `Ctrl-p`: Go back to a previous command, replacing `↑` key. Think p as in **p**revious.
* `Ctrl-n`: Go to the next command letter forward, replacing `↓` key. Think n as in **n**ext.

* `Ctrl-r`: Reverse search history commands containing certain key word.
<!-- http://stackoverflow.com/questions/12373586/how-to-reverse-i-search-back-and-forth -->
* `Ctrl-s`: Forward search history commands containing certain key word. This
  is the opposite of `Ctrl-r`, if it doesn't work. Try executing `stty -ixon`
  first, then if it works, put `stty -ixon` in your `~/.bashrc`.

I think that's all what you need, with more practise, you're well on your way
to become an expert in shell.

### Case manipulation:

* `Alt-c`: **C**apitalize a word
* `Alt-u`: **U**ppercase a word
* `Alt-l`: **L**owercase a word

### Space manipulation:

* `Alt-\`: Remove all continuous spaces starting from the current position of
  the marker on both sides.

### Bonus:

If you happens to use [Emacs](https://www.gnu.org/software/emacs/) as your
favorite editor, most of the above commands (except `Ctrl-u` as far as I am
aware of) applies to navigation and operations in the Emacs buffer, too. So that's
killing two birds with one stone!

Of course, in Emacs, you will need to navigate more than a single line, e.g.
paragraphs, pages, or the whole file. These commands are all available online,
but may be a bit scattered. I will summarize those most handy ones to me in a
later post.

Cheers.
