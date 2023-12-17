---
layout: post
title: Coefficient of determination ($R^2$) and Pearson correlation coefficient ($\rho$) are equivalent in OLS
author: Zhuyi Xue
tags: machine learning
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
});
</script>

This post proves $R^2 = \rho^2$ in ordinary least squares (OLS).

In [a previous post on
$R^2$](https://zyxue.github.io/2023/12/17/properties-of-r-squared-OLS.html), we
derived for OLS,

$$
\begin{align*}
R^2 &= \frac{Var(f)}{Var(y)} = \frac{\sigma_f^2}{\sigma_y^2} \\
\bar{y} &= \bar{f}
\end{align*}
$$

The Pearson correlation coefficient is defined as

$$
\begin{align*}
\rho
&= \frac{\text{Cov}(y, f)}{\sigma_y \sigma_f} \\
\end{align*}
$$

We'd like to prove $\text{Cov}(y, f) = \sigma_f^2$, i.e. the correlation is
equal to the variance of $f$.

$$
\begin{align}
\text{Cov}(y, f)
&= \frac{1}{N} (\mathbf{y} - \bar{\mathbf{y}})^T (\mathbf{f} - \bar{\mathbf{f}}) \\
&= \frac{1}{N} \left( \mathbf{y}^T \mathbf{f} - \mathbf{y}^T \bar{\mathbf{f}} - \bar{\mathbf{y}}^T \mathbf{f} + \bar{\mathbf{y}}^T \bar{\mathbf{f}} \right) \label{eq:before_replacement} \\
&= \frac{1}{N} \left( \mathbf{f}^T \mathbf{f} - \mathbf{f}^T \bar{\mathbf{f}} - \bar{\mathbf{f}}^T \mathbf{f} + \bar{\mathbf{f}}^T \bar{\mathbf{f}} \right) \label{eq:after_replacement} \\
&= \frac{1}{N} (\mathbf{f} - \bar{\mathbf{f}})^T (\mathbf{f} - \bar{\mathbf{f}}) \\
&= \sigma_f^2
\end{align}
$$

From Eq. \eqref{eq:before_replacement} to Eq. \eqref{eq:after_replacement}, the
replacement of the first item is because

$$
\begin{align*}
\mathbf{f}^T \mathbf{f}
&= \left( H^T \mathbf{y} \right)^T H \mathbf{y} \\
&= \mathbf{y}^T H^T H \mathbf{y} \\
&= \mathbf{y}^T H \mathbf{y} \\
&= \mathbf{y}^T \mathbf{f}
\end{align*}
$$

where $H = X(X^T X)^{-1} X^T$. And the replacement of the other threee items all
used the fact $\bar{y} = \bar{f}$.

Thefore,

$$
\begin{align}
\rho^2
&= \frac{\text{Cov}(f, x)^2}{\sigma_y^2 \sigma_f^2} \\
&= \frac{\sigma_f^4}{\sigma_y^2 \sigma_f^2} \\
&= \frac{\sigma_f^2}{\sigma_y^2} \\
&= R^2
\end{align}
$$

Here is a [notebook](https://nbviewer.jupyter.org/github/zyxue/sutton-barto-rl-exercises/blob/master/stats/relationship-between-coefficient-of-determination-and-pearson-correlation-coefficient.ipynb) I wrote demonstrating this result.
