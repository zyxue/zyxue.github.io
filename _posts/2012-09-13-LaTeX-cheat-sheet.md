---
layout: post
title:  LaTeX Cheat Sheet
author: Zhuyi Xue
tags: LaTeX
---

### Font Sizes

{% highlight latex %}
\tiny
\scriptsize
\footnotesize
\small
\normalsize
\large
\Large
\LARGE
\huge
\Huge
{% endhighlight %}

### Arrow styles available in `Tikz` ([pgmanual.pdf](http://www.texample.net/media/pgf/builds/pgfmanualCVS2012-11-04.pdf))

{% highlight latex %}
latex
latex'
stealth
triangle 90
triangle 45
angle 90
angle 60
{% endhighlight %}

### Space in math

{% highlight latex %}
\; a thick space
\: a medium space
\, a thin space
\! a negative thin space
{% endhighlight %}

### Labeling

{% highlight latex %}
chap: chapter
sec: section
fig: figure
tab: table
eq: equation
lst: code listing
itm: enumerated list item
{% endhighlight %}

### Line drawing, see [pgfmanual.html](http://stuff.mit.edu/afs/athena/contrib/tex-contrib/beamer/pgf-1.01/doc/generic/pgf/version-for-tex4ht/en/pgfmanualse9.html).

### Relative position of the object `s`

{% highlight latex %}
(s.north west)
(s.input 1)
(s.west)
(s.mid west) (s.text)
(s.north east)
(s.30)
(s.east) (s.output)
(s.mid)(s.mid east)
(s.base west)
(s.input 2) (s.base) (s.base east)
(s.south west) (s.south) (s.south east)
{% endhighlight %}

### Examples for defining `.styles`

{% highlight latex %}
block_left/.style = {
  rectangle, draw=black, thick, fill=white, text width=16em,
  text ragged, minimum height=4em, inner sep=6pt
},
block_noborder/.style = {
  rectangle, draw=none,  thick, fill=none,  text width=18em,
  text centered, minimum height=1em
},
block_assign/.style = {
  rectangle, draw=black, thick, fill=white, text width=18em,
  text ragged, minimum height=3em, inner sep=6pt
},
block_lost/.style = {
  rectangle, draw=black, thick, fill=white, text width=16em,
  text ragged, minimum height=3em, inner sep=6pt
},
{% endhighlight %}

### Learned from [here](http://www.physicsforums.com/showthread.php?t=229223)

Use `\mathbf` in math mode. e.g.

`\mu` v.s. `\mathbf\mu`:

$$ \mu  \;\;\; \mathbf \mu$$

One drawback of `\mathbf`: Its not in italics.

`a` v.s. `\mathbf a`:

$$ a \;\;\; \mathbf a$$

If you have the AMS math package (`\usepackage{amssymb,amsmath}`), `\boldsymbol`
puts things in bold italics.

`\mu` v.s. `\mathbf\mu` v.s. `\boldsymbol\mu`

$$ \mu \;\;\; \mathbf \mu \;\;\; \boldsymbol \mu $$

`a` v.s. `\mathbf a` versus `\boldsymbol a`:

$$ a \;\;\; \mathbf a\;\;\; \boldsymbol a $$


### Learned from [here](http://nw360.blogspot.ca/2006/11/latex-headheight-is-too-small.html)

LaTeX: `\headheight` is too small

You never know what you will meet when you use LaTeX, luckily, we can search
Google or Yahoo to find the answer. However, it cannot help all the times. There
is a problem you may come across using package `fancyhdr`:

{% highlight default %}
Package Fancyhdr Warning: \headheight is too small (12.0pt): Make it at least 14.49998pt.
{% endhighlight %}

We now make it that large for the rest of the document. It was not easy to find
the answer via either Google or Yahoo. Here is the solution. Insert

{% highlight latex %}
\setlength{\headheight}{15pt}
{% endhighlight %}

in the preamble, i.e. before `\begin{document}`.

Tips:

* `\headheight` defines the height of the header.
* `\setlength{len-cmd}{length}`. The `\setlength` command is used to set the
  value of a length command, `len-cmd`, which is specified as the first
  argument. The length argument can be expressed in any terms of length LaTeX
  understands, i.e., inches (`in`), millimeters (`mm`), points (`pt`), etc.

### Learned from [here](http://pleasemakeanote.blogspot.ca/2010/06/how-to-activate-subsubsubsection-in.html)

How to Activate `\subsubsubsection` in LaTeX?

The equivalent to `\subsubsubsection` is `\paragraph`. To have that numbered
simply use:

{% highlight latex %}
\setcounter{secnumdepth}{5}
{% endhighlight %}

then:

{% highlight latex %}
\section{} % level 1
\subsection{} % level 2
\subsubsection{} % level 3
\paragraph{} % level 4 - equivalent to subsubsubsection
\subparagraph{} % level 5
{% endhighlight %}

### Another useful site on [Displayed Math](http://www.math.uiuc.edu/~hildebr/tex/displays.html)
