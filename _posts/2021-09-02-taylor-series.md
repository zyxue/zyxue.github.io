---
layout: post
title: Taylor series
author: Zhuyi Xue
tags: statistics
---

Memo for Taylor series, approximating $f(x)$ from $a$.

**General form**:

$$
\begin{align}
f(x) \approx g(x)
&= f(a) + f'(a)(x - a) + \frac{f''(a)}{2!}(x - a)^2 + \frac{f^{(3)}(a)}{3!}(x - a)^3 + \cdots + \frac{f^{(n)}(a)}{n!}(x - a)^n + \cdots \\
&= \sum_{i=0}^n \frac{1}{i!} \frac{d^i f(a)}{d x^i} (x - a)^i
\end{align}
$$

The intuition behind it is that when we want to approximate a function $f(x)$
with an n-th order ($n \ge 0$) polynomial $g(x)$, and we'd want the n-th order
derivatives of $g(x)$ and $f(x)$ at $x=a$ to be equal.

**Maclaurin series ($a = 0$)**:

$$
\begin{equation}
f(x) = f(0) + f'(0)x + \frac{f''(0)}{2!}x^2 + \frac{f^{(3)}(0)}{3!}x^3 + \cdots + \frac{f^{(n)}(0)}{n!}x^n + \cdots
\end{equation}
$$


Equivalently, if you consider $x = a + \Delta x$, then the Taylor series can be written as

$$
\begin{equation}
f(x) = f(a + \Delta x) = f(a) + f'(a)\Delta x + \frac{f''(a)}{2!}\Delta x^2 + \frac{f^{(3)}(a)}{3!}\Delta x^3 + \cdots + \frac{f^{(n)}(a)}{n!}\Delta x^n + \cdots
\end{equation}
$$
