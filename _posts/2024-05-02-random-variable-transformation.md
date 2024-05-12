---
layout: post
title: Random variable transformation
author: Zhuyi Xue
tags: probability
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
});
</script>

Question:

Given r.v. $X$ with cdf $F(X)$ with pdf $f_X(x)$, $Y = g(X)$, derve the pdf for
$Y$. Here, we assume $g$ is monotonically increasing, and both $g^{-1}(y)$ and
its derivative wrt. $x$ exists. Since $g$ is monotonically increasing, so $dg^{-1}/dy \ge 0$

$$
\begin{align}
F_Y(y)
&= \mathbb{P}(Y \le y) \\
&= \mathbb{P}(Y \le g(x)) \\
&= \mathbb{P}(X \le g^{-1}(y)) \\ \label{eq:Y_to_X_v1}
&= F_X(g^{-1}(y)) \\
\frac{d F_Y}{dy}
&= \frac{F_X(g^{-1}(y))}{dx} \frac{dx}{dy} \\
f_Y(y)
&= f_X(g^{-1}(y)) \frac{dg^{-1}(y)}{dy}
\end{align}
$$

Note,

* Eq. $\eqref{eq:Y_to_X_v1}$ relies on $g$ being monotonically increasing, so
  $g^{-1}(y)$ is also monotonically increasing.


In the case when $g(x)$ is monotonically decreasing, then $g^{-1}(x)$ is also
monotonically decreasing, and $dg^{-1}/dy \le 0$, which is quite similar

$$
\begin{align*}
F_Y(y)
&= \mathbb{P}(Y \le y) \\
&= \mathbb{P}(Y \le g(x)) \\
&= \mathbb{P}(X \ge g^{-1}(y)) \\ \label{eq:Y_to_X_v2}
&= 1 - F_X(g^{-1}(y)) \\
\frac{d F_Y}{dy}
&= - \frac{F_X(g^{-1}(y))}{dx} \frac{dx}{dy} \\
f_Y(y)
&= - f_X(g^{-1}(y)) \frac{dg^{-1}(y)}{dy}
\end{align*}
$$

Merging the two cases together, we have

$$
\begin{align*}
f_Y(y)
&= f_X(g^{-1}(y)) \left| \frac{dg^{-1}(y)}{dy} \right|
\end{align*}
$$