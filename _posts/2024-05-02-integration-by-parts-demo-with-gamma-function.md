---
layout: post
title: Integration by parts demo with Gamma function
author: Zhuyi Xue
tags: probability
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
});
</script>

* toc
{:toc}

## Derivative of product

Suppose $u=u(x), v=v(x)$. By definition of derivative:

$$
\begin{align}
(uv)'
&= \lim_{\delta \rightarrow 0} \frac{u(x + \delta) v(x + \delta) - u(x)v(x)}{\delta} \\
&= \lim_{\delta \rightarrow 0} \left[ \frac{u(x + \delta) v(x + \delta) - u(x)v(x + \delta)}{\delta} + \frac{u(x)v(x + \delta) - u(x)v(x)}{\delta} \right ] \label{eq:minus_plus_trick} \\
&= \lim_{\delta \rightarrow 0} \left [ \frac{u(x + \delta) v(x + \delta) - u(x)v(x + \delta)}{\delta} +  \frac{u(x)v(x + \delta) - u(x)v(x)}{\delta} \right ] \\
&= \lim_{\delta \rightarrow 0} \left [\frac{(u(x + \delta) - u(x)) v(x + \delta)}{\delta} +  \frac{u(x)(v(x + \delta) - v(x))}{\delta} \right ] \\
&= u'v + uv'
\end{align}
$$

Note in Eq. $\eqref{eq:minus_plus_trick}$, we simply minus $u(x)v(x + \delta)$
and then added it back in the numerator.

Therefore, we have the [product rule](https://en.wikipedia.org/wiki/Product_rule):

$$
\begin{align}
(uv)' = u'v + uv' \label{eq:product_rule}
\end{align}
$$

## Integration by parts
Eq. $\eqref{eq:product_rule}$ can be rearranged into

$$
\begin{align}
uv' &= (uv)' - u'v \\
\int uv' dx &= \int (uv)' dx - \int u'v dx \label{eq:integration_by_parts} \\
\end{align}
$$

Eq. $\eqref{eq:integration_by_parts}$ is usally referred to as [integration by
parts](https://en.wikipedia.org/wiki/Integration_by_parts). It is useful when
integrating the product of two functions, e.g. $a(x) = u, b(x) = v$, esp. $uv'$
is hard to integrate, but $(uv)'$ and $u'v$ is easy to integrate. We'll
demonstrate such a use case with the proof for the factorial property of the
Gamma function.

## Gamma function generalizes factorial

The gamma function is defined as

$$
\begin{align}
\Gamma(n)
&=\int_0^{\infty} x^{n - 1} e^{-x} dx
\end{align}
$$

Note, the parameter for $\Gamma(n)$ is $n$, not $x$, which is just a dummy
variable that will be integrated out. $\Gamma(n)$ is a generalization fo the
factorial function $n!$, and it has the factorial property

$$
\begin{align}
\Gamma(n + 1) = n \Gamma(n)
\end{align}
$$

Let's prove it. Note, integrating

$$
\begin{align}
\Gamma(n + 1) = \int_0^{\infty} x^n e^{-x} dx
\end{align}
$$

is hard, but if we consider

$$
\begin{align}
u &= x^n \\
v' &= e^{-x} \\
\end{align}
$$

then, we have

$$
\begin{align}
u' &= n x^{n - 1} \\
v &= - e^{-x} \\
\end{align}
$$

Then, applying integration by parts (Eq. $\eqref{eq:integration_by_parts}$),
both $uv$ and $u'v$ are easy to integrate.

$$
\begin{align}
\Gamma(n + 1)
&= \int_{0}^{\infty} x^n e^{-x} dx \\
&= \left[ - x^n e^{-x}  \right ]_{0}^{\infty} + n \int_0^{\infty} x^{n-1} e^{-x} dx \\
&= 0 + n \Gamma(n) \\
&= n \Gamma(n) \\
\end{align}
$$

Alternatively, given $x^n$ is equally easy to integrate, we can also set

$$
\begin{align}
u &= e^{- x} \\
v' &= x^n \\
u' &= - e^{- x} \\
v &= \frac{1}{n + 1} x^{n + 1}
\end{align}
$$

and

$$
\begin{align}
\Gamma(n+1)
&= \int_{0}^{\infty} x^n e^x dx \\
&= \left[ - x^n e^x  \right ]_{0}^{\infty} + \frac{1}{n+1} \int_0^{\infty} x^{n+1} e^x dx \\
(n + 1) \Gamma(n+1)
&= 0 +  \Gamma(n + 2) \\
\Gamma(n + 2)
&= (n + 1) \Gamma(n+1) \\
\end{align}
$$

which is equivalent to $\Gamma(n) = n\Gamma(n - 1)$.