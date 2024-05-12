---
layout: post
title: Moment generating function
author: Zhuyi Xue
tags: statistics
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
});
</script>

* toc
{:toc}

## Definition

Given a random variable $X$, its moment generating function (MGF) is defined as

$$
\begin{align}
M_X(t) = \mathbb{E}\left[e^{tX}\right]
\end{align}
$$

### Prove $\mathbb{E}\left[X^k\right] = \mathbb{E}\left[M_X^{(k)}(0) \right]$

Superscript $^(k)$ means the kth derivative.

This property says that the $k$th moment of $X$ is equal to the expectation of
the $k$th derivative wrt. $t$ at $t=0$.

**Proof**:

Given,

$$
\begin{align}
M_X^{(1)}(t)
&= \frac{d M_X(t)}{dt} = \mathbb{E}\left[ X e^{tX}\right] \\
M_X^{(2)}(t)
&= \frac{d^2 M_X(t)}{dt^2} = \mathbb{E}\left[ X^2 e^{tX}\right] \\
\vdots \\
M_X^{(k)}(t)
&= \frac{d^k M_X(t)}{dt^k} = \mathbb{E}\left[ X^k e^{tX}\right] \\
\end{align}
$$

so

$$
\begin{align}
M_X^{(1)}(0)
&= \mathbb{E}\left[ X \right] \\
M_X^{(2)}(0)
&= \mathbb{E}\left[ X^2 \right] \\
\vdots \\
M_X^{(k)}(0)
&= \mathbb{E}\left[ X^k \right] \\
\end{align}
$$

### Prove $M_{aX + b}(t) = e^{bt}M_X(at)$

$$
\begin{align}
M_{aX + b}(t)
&= \mathbb{E}\left[ e^{t(aX + b)} \right] \\
&= e^{bt} \mathbb{E}\left[ e^{atX)} \right] \\
&= e^{bt} M_X(at) \\
\end{align}
$$

### Prove $M_{X + Y}(t) = M_X(t) M_Y(t)$

Suppose $X$ and $Y$ are independent random variables, then

$$
\begin{align}
M_{X + Y}(t)
&= \mathbb{E}\left[ e^{t(X + Y)} \right] \\
&= \mathbb{E}\left[ e^{tX} e^{tY} \right] \\
&= \iint_{x, y} e^{tX} e^{tY} f(X=x, Y=y) dx dy \\
&= \int_x e^{tX} f(X=x) dx \int_y e^{tY} (Y=y) dy  \\
&= \mathbb{E}\left[ e^{tX} \right] \mathbb{E}\left[ e^{tY} \right] \\
&= M_X(t)M_Y(t) \\
\end{align}
$$