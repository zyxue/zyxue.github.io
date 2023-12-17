---
layout: post
title: Property of $R^2$ in OLS
author: Zhuyi Xue
tags: machine learning, boosting
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
});
</script>

* toc
{:toc}

# Defintion of $R^2$

Suppose we have data $X$ of $N$ examples, $(\mathbf{x}_1, y_1), \cdots,
(\mathbf{x}_N, y_N)$,  and fit a predictive model $f(\mathbf{x})$. We apply $f$
to the data to obtain $f(\mathbf{x}_i), \cdots, f(\mathbf{x}_N)$ For simplicity,
we denote $f_i = f(\mathbf{x}_i)$.

$R^2$, aka. coefficient of determination, is defined as

$$
\begin{align}
    R^2 = 1 - \frac{\sum_i (y_i - f_i)^2}{\sum_i (y_i - \bar{y})^2} = 1 - \frac{RSS}{TSS}
\end{align}
$$

where

* $\bar{y} = \frac{1}{N} \sum_i y_i$ is mean of labels.
* $RSS$ is called the **residual sum of squares**.
* $TSS$ is called the **total sum of squares**, aka. $y$ variance.

# In OLS, $\bar{y} = \bar{f}$ and $ESS = Var(f)$

Following the naming convention of $RSS$ and $TSS$, We define

$$ESS = \sum_i(f_i - \bar{y})^2$$

as the **explained sum of squares**. Note, $f_i$ is subtracted by the mean of
labels ($\bar{y}$) instead of predictions ($\bar{f}$). However, in the ordinary least
squares (OLS) given

$$
\begin{align}
\mathbf{f}
&= X (X^T X)^{-1} X^T \mathbf{y} \label{eq:OLS} \\
\mathbf{y} - \mathbf{f}
&= (I - X (X^T X)^{-1} X^T) \mathbf{y} \\
X^T(\mathbf{y} - \mathbf{f})
&= X^T(I - X (X^T X)^{-1} X^T) \mathbf{y} \\
&= (X^T - X^T) \mathbf{y} \\
&= \mathbf{0} \\
\end{align}
$$

See
[here](https://zyxue.github.io/2021/09/08/analytical-solutions-to-different-sum-of-squares-loss-functions-for-linear-regression.html)
for a proof of Eq. \eqref{eq:OLS}. $\mathbf{f} = (f_1, \cdots, f_N)^T$, $\mathbf{y} = (y_1, \cdots, y_N)^T$.

Because the first column of $X$ is all ones, so we also have

$$
\begin{align}
\mathbf{1} &^T (\mathbf{y} - \mathbf{f}) = 0 \label{eq:reason_to_y_bar_equals_f_bar}
\end{align}
$$

which is equivalent to $\bar{f} = \bar{y}$. In other words, $ESS = Var(f)$ in OLS.

# In OLS, TSS = RSS + ESS

From the above sections, we have obtained

$$
\begin{align}
TSS
&= \sum_i (y_i - \bar{y})^2 \\
RSS
&= \sum_i (y_i - f_i)^2 \\
ESS
&= \sum_i (f_i - \bar{y})^2 \\
\bar{y}
&= \bar{f}
\end{align}
$$

Note $\bar{y} = \bar{f}$ is OLS-specific, it may not hold in other models.

Proof for $TSS = RSS + ESS$:

$$
\begin{align}
RSS + ESS
&= \sum_i (y_i - f_i)^2 + \sum_i (f_i - \bar{y})^2 \\
&= \sum_i (y_i - f_i + f_i - \bar{y})^2 - 2 \sum_i (y_i - f_i)(f_i - \bar{y})  \\
&= ESS - 2 \sum_i (y_i - f_i)(f_i - \bar{y})  \\
\end{align}
$$

so we just need to prove the second term is equal to 0,

$$
\begin{align}
\sum_i (y_i - f_i)(f_i - \bar{y})
&= (\mathbf{y} - \mathbf{f})^T(\mathbf{f} - \bar{\mathbf{y}}) \\
&= \mathbf{y}^T \mathbf{f} - \mathbf{f}^T \mathbf{f} - \mathbf{y}^T \bar{\mathbf{y}} + \mathbf{f}^T  \bar{\mathbf{y}} \\
&= \Big(\mathbf{y}^T \mathbf{f} - \mathbf{f}^T \mathbf{f} \Big) + \Big(\mathbf{y}^T \bar{\mathbf{y}} - \mathbf{f}^T  \bar{\mathbf{y}} \Big) \label{eq:grouped} \\
&= 0 + 0  \\
&= 0
\end{align}
$$

* $\bar{\mathbf{y}} = \mathbf{1} \bar{y}$
* That the first part of Eq. $\eqref{eq:grouped}$ is equal to 0 is easily obtained by replacing $\mathbf{f}$ with Eq. $\eqref{eq:OLS}$.
* That the second part is equal to 0 is a direct result of Eq. $\eqref{eq:reason_to_y_bar_equals_f_bar}$.

Therefore, in OLS, $R^2$ can also be rewritten as

$$
\begin{align}
R^2
&= \frac{ESS}{TSS} \\
&= \frac{\sum_i (f_i - \bar{y})^2 }{\sum_i (y_i - \bar{y})^2} \\
&= \frac{\sum_i (f_i - \bar{f})^2 }{\sum_i (y_i - \bar{y})^2} \\
&= \frac{Var(f)}{Var(y)} \\
\end{align}
$$

and intuitively interpreted as the the amount of variance in $y$ that can be
explained by the model $f$.
