---
layout: post
title: Taylor series
author: Zhuyi Xue
tags: statistics
---

Memo for Taylor series, approximating $f(x)$ from $a$.

**General form**

$$
\begin{align}
g(x)
&= f(a) + f'(a)(x - a) + \frac{f''(a)}{2!}(x - a)^2 + \frac{f^{(3)}(a)}{3!}(x - a)^3 + \cdots + \frac{f^{(n)}(a)}{n!}(x - a)^n + \cdots \label{eq:taylor_general} \\
&= \sum_{i=0}^n \frac{1}{i!} f^{(i)}(a) (x - a)^i \\
&\approx f(x)
\end{align}
$$

The intuition is that when approximating a function $f(x)$ with a polynomial
$g(x)$ starting from $x=a$, we'd like keep

$$
g^{(i)}(a) = f^{(i)}(a)
$$

for all $i = 1, 2, \cdots$. That is to keep the i-th derivatives of $g$ and $f$
at $x=a$ equal. This property is easy to verify for Eq. $\eqref{eq:taylor_general}$.

An **equivalent form** of the above is to replace $x$ with $a + \Delta x$, given
we are expanding $g(x)$ around $a$, then the Taylor series can be written as

$$
\begin{equation}
f(x) = f(a + \Delta x) = f(a) + f'(a)\Delta x + \frac{f''(a)}{2!}\Delta x^2 + \frac{f^{(3)}(a)}{3!}\Delta x^3 + \cdots + \frac{f^{(n)}(a)}{n!}\Delta x^n + \cdots
\end{equation}
$$

**Maclaurin series**

When $a = 0$, i.e. approximating $f(x)$ starting from $0$, the corresponding
Taylor series is also called Maclaurin series.

$$
\begin{equation}
f(x) = f(0) + f'(0)x + \frac{f''(0)}{2!}x^2 + \frac{f^{(3)}(0)}{3!}x^3 + \cdots + \frac{f^{(n)}(0)}{n!}x^n + \cdots
\end{equation}
$$

When $f(x) = e^x$, Maclaurin has a very neat form,

$$
\begin{align}
e^x
&= \frac{x^0}{0!} + \frac{x^1}{1!} + \frac{x^2}{2!} + \frac{x^3}{3!} + \cdots \\
&= \sum_{t=0}^\infty \frac{x^t}{t!} \\
&= 1 + x + \frac{x^2}{2} + \cdots \\
\end{align}
$$


**Multi-dimensional form**

When $\mathbf{x}$ is a vector, Taylor series can be written as

$$
\begin{equation}
g(\mathbf{x})
= f(\mathbf{a}) + \nabla f(\mathbf{\mathbf{a}})^T (\mathbf{x} - \mathbf{a}) + \frac{1}{2!} (\mathbf{x} - \mathbf{a})^T \nabla^2 f(\mathbf{a}) (\mathbf{x} - \mathbf{a})  + \cdots \\
\end{equation}
$$

The $\nabla^i f(\mathbf{x})$ with $i > 2$ results in a tensor.
