---
layout: post
title: Relationship between R squared and Pearson correlation coefficient
author: Zhuyi Xue
tags: machine learning, boosting
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
});
</script>

<span style="color:red">THE DERIVATION IN THIS POST IS <i>INCORRECT</i>, PLEASE SEE [AN
UPDATED
ONE](https://zyxue.github.io/2023/12/17/coefficient-of-determination-and-pearson-correlation-coefficient-are-equivalent-in-OLS.html)
INSTEAD</span>.

# Coefficient of determination (aka. $R^2$)

Consider the ordinary least square (OLS) model:

$$
\begin{equation}
    y = \mathbf{X} \beta + \epsilon
    \label{eq:OLS}
\end{equation}
$$

After fitting the model to the data  ($\mathbf{X}$, $y$), let

$$\hat y = \mathbf{X} \beta$$

We would like to understand the relationship between the variance of $y$ and that of $\hat y$.

$$
\begin{align*}
\mathbb{V}[y]
&= \mathbb{V}[\hat y + \epsilon] \\
& = \mathbb{V}[\hat y] + \mathbb{V}[\epsilon] + 2\text{Cov}(\hat y, \epsilon) \\
&= \mathbb{V}[\hat y] + \mathbb{V}[\epsilon]
\end{align*}
$$

See proof for the second equality in the Supplemental. The third equation is because $\hat y$ and $\epsilon$ are independent random variables, thus their covariance is 0.

Define

$$
\begin{align*}
R^2
&= \frac{\mathbb{V}[\hat y]}{\mathbb{V}[y]} = \frac{\sum_i^n (\hat y_i - \bar{\hat y})^2}{\sum_i^n (y_i - \bar y)^2}  \\
\end{align*}
$$

Therefore, $R^2$ measures the ratio of $\mathbb{V}[\hat y]$ over $\mathbb{V}[y]$, and it's commonly interpreted as the the amount of variance in $y$ that can be explained by the OLS model.

# Pearson correlation coefficient

\begin{equation}
    \rho = \frac{\text{Cov}(y, \hat y)}{\sigma_y \sigma_{\hat y}} \label{eq:pearson_corr}
\end{equation}

See definition on [Wikipedia](https://en.wikipedia.org/wiki/Pearson_correlation_coefficient).

# Relationship between $\rho$ and $R^2$

Now we've defined both coefficient of determination and Pearson correlation coefficient, let's see their relationship.

Note

$$
\begin{align*}
\text{Cov}(y, \hat y)
&= \text{Cov}((\hat y + \epsilon), \hat y) \\
&= \text{Cov}(\hat y, \hat y) +  \text{Cov}(\epsilon, \hat y) \\
&= \mathbb{V}(\hat y)
\end{align*}
$$

See proof for the second equality in the Supplemental. So

$$
\begin{align*}
\rho^2
&= \frac{\mathbb{V}[\hat y]^2}{\mathbb{V}[y] \mathbb{V}[\hat y]} = \frac{\mathbb{V}[\hat y]}{\mathbb{V}[y]}  = R^2
\end{align*}
$$

Therefore, $\rho^2 = R^2$, neat!

Here is a [notebook](https://nbviewer.jupyter.org/github/zyxue/sutton-barto-rl-exercises/blob/master/stats/relationship-between-coefficient-of-determination-and-pearson-correlation-coefficient.ipynb) I wrote demonstrating this result.

# Supplemental

Note it's straightforward to prove

$$
\begin{align}
    \text{Cov}(X, Y)
    &= \mathbb{E}[(X - \mathbb{E}[X])(Y - \mathbb{E}[Y])] \nonumber \\
    &= \mathbb{E}[XY]) - \mathbb{E}[X]\mathbb{E}[Y]
    \label{eq:cov2exp}
\end{align}
$$

This fact will be used in both of the two proofs below.

### Proof for $\mathbb{V}[X + Y] = \mathbb{V}[X]  + \mathbb{V}[Y] + 2\text{Cov}(X, Y)$

$$
\begin{align*}
\mathbb{V}[X + Y]
&= \mathbb{E}[(X + Y)^2] - \mathbb{E}[X + Y]^2 \\
&= \mathbb{E}[(X + Y)^2] - (\mathbb{E}[X] + \mathbb{E}[Y])^2 \\
&= \mathbb{E}[X^2]  + \mathbb{E}[Y^2] + \mathbb{E}[2XY] - \mathbb{E}[X]^2 - \mathbb{E}[Y]^2 - 2\mathbb{E}[X]\mathbb{E}[Y] \\
&= \left( \mathbb{E}[X^2] - \mathbb{E}[X]^2 \right) + \left(\mathbb{E}[Y^2] -  \mathbb{E}[Y]^2  \right ) + \left ( 2(\mathbb{E}[XY]) - \mathbb{E}[X]\mathbb{E}[Y] \right)\\
&= \mathbb{V}[X]  + \mathbb{V}[Y] + 2\text{Cov}(X, Y)
\end{align*}
$$

The 4th equality used Eq. $\eqref{eq:cov2exp}$.

### Proof for $\text{Cov}(X, (Y + Z)) = \text{Cov}(X, Y) + \text{Cov}(X, Z)$

$$
\begin{align*}
\text{Cov}(X, (Y + Z))
&= \mathbb{E}[X(Y + Z)] - \mathbb{E}[X] \mathbb{E}[Y + Z] \\
&= \mathbb{E}[XY] + \mathbb{E}[XZ]  - \mathbb{E}[X] \mathbb{E}[Y] - \mathbb{E}[X] \mathbb{E}[Z] \\
&= \big( \mathbb{E}[XY] - \mathbb{E}[X] \mathbb{E}[Y] \big) + \big( \mathbb{E}[XZ] - \mathbb{E}[X] \mathbb{E}[Z] \big) \\
&= \text{Cov}(X, Y) + \text{Cov}(X, Z)
\end{align*}
$$

The 1st and 4th equalities used Eq. $\eqref{eq:cov2exp}$.

# References:

* https://economictheoryblog.com/2014/11/05/the-coefficient-of-determination-latex-r2/
* https://economictheoryblog.com/2014/11/05/proof/
