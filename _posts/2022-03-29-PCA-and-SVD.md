---
layout: post
title:  PCA and SVD
author: Zhuyi Xue
tags: linear_algebra
---

In this post, I describe how principal component analysis (PCA) works, and how
singular value decomposition (SVD) can be used to conduct PCA.

## PCA

Suppose we have a dataset represented by a $d \times n$ matrix $X$,

$$
\begin{align}
\underset{d \times n}{X}
= \begin{bmatrix}
| & \cdots & | \\
\mathbf{x}^{(1)} & \cdots & \mathbf{x}^{(n)} \\
| & \cdots & |
\end{bmatrix}
= \begin{bmatrix}
x_1^{(1)} & \cdots & x_1^{(n)} \\
\vdots & \ddots & \vdots \\
x_d^{(1)} & \cdots & x_d^{(n)}
\end{bmatrix}
\end{align}
$$

where

* $d$ is the dimension of the feature space, and
* $(n)$ is the number of data points.

**PCA looks for orthogonal unit vectors in the feature space that, when $X$ are
projected onto them, the variances are maximized.**

When a vector $\mathbf{x}$ is projected onto a unit vector $\mathbf{u}$, the
coordinate of the projected vector is $\mathbf{u}^T \mathbf{x}$ (See Appendices
for a proof), so we formulate the optimization problem

$$
\begin{align}
\max_{\mathbf{u}} \quad & \text{Var}(\mathbf{u}^T X)  = \mathbf{u}^T S \mathbf{u} \\
\text{s.t.} \quad & \mathbf{u}^T \mathbf{u} = 1
\end{align}
$$

Note

* $\mathbf{u}^T X$ is a vector of all coordinates when all column vectors in $X$
  are projected onto $\mathbf{u}$, and $\text{Var}(\mathbf{u}^T X)$ is the
  sample variance of these coordinates.
* $\text{Var}(\mathbf{u}^T X)$ is equal to $\mathbf{u}^T S \mathbf{u}$, where
  $S$ is the sample covariance matrix (see Appendices for a proof).
* this is not an convex optimization problem because we maximizing a convex
  function instead of minimizing it.

To maximize $\mathbf{u}^T S \mathbf{u}$, we form the Lagrangian,

$$
\begin{align}
\mathcal{L}
&= \mathbf{u}^T S \mathbf{u} - \lambda (\mathbf{u}^T \mathbf{u}) \\
\end{align}
$$

Take the derivative wrt. $\mathbf{u}$ and set it to zero,

$$
\begin{align}
\frac{\partial \mathcal{L}}{\partial \mathbf{u}}
= 2 S \mathbf{u} - 2\lambda \mathbf{u}
&= 0\\
S\mathbf{u}
&= \lambda \mathbf{u}
\end{align}
$$

As seen, the $\mathbf{u}$ that maximizes the variance of $\mathbf{u}^T X$ is an
eigenvector of $S$, so the maximum can be written as

$$
\begin{align}
\mathbf{u}^T S \mathbf{u} = \mathbf{u}^T \lambda \mathbf{u} = \lambda
\end{align}
$$

that is **the eigenvalues of $S$ are the (local) maximum variances of the
projected coordinates**. If we sort the eigenvalues in descending order, of all
the projected coordinates, the (global) maximum variance is $\lambda_1$, the
corresponding eigenvector $\mathbf{u}_1$ is called the first principal component
of $X$.

To find the second principal component, we formulate a second optimization
problem.

$$
\begin{align}
\max_{\mathbf{u}} \quad & \text{Var}(\mathbf{u}^T X)  \\
\text{s.t.} \quad & \mathbf{u}^T \mathbf{u} = 1 \\
            \quad & \mathbf{u} \perp \mathbf{u}_1
\end{align}
$$

Note the second eigenvalue $\lambda_2$ is also a (local) maximum variance with
the corresponding eigenvector $\mathbf{u}_2$ orthogonal to $\mathbf{u}_1$
because $S$ is a symmetric matrix, so ($\lambda_2$, $\mathbf{u}_2$) is a
solution to the second optimization problem.

Similarly, to find the $k$th principal component, the corresponding optimization
problem is

$$
\begin{align}
\max_{\mathbf{u}} \quad & \text{Var}(\mathbf{u}^T X)  \\
\text{s.t.} \quad & \mathbf{u}^T \mathbf{u} = 1 \\
            \quad & \mathbf{u} \perp \mathbf{u}_j, j = 1, \cdots, k-1
\end{align}
$$

The ($\lambda_k$, $\mathbf{u}_k$) of $S$ is a solution.

When $X$ is of rank $r$, the covariance matrix will also be of rank $r$ (See
Appendices for a proof), there are at most $r$ principal components. All the
principal components together would explain all the variances of the data
because

$$
\begin{align}
\text{Tr}(S) &= \sum_{i=1}^r \lambda_i
\end{align}
$$

That is the trace of the sample covariance matrix, i.e. the sum of variances
along all original coordinates, is equal to the sum of eigenvalues, i.e. the sum
of variances along the projected coordinates.

For a typical dataset where $n > p$,

* when there is no colinearity among features, $r=p$.
* when there is colienarity, $r < p$.

When $n < p$, e.g. for a typical gene expression data, $r = n$.

In practice, we usually take $k < r$ principal components.

In summary, to conducting PCA for a dataset $X$,

1. Calculate the sample covariance matrix $S$.
1. Apply eigendecomposition to $S$, sort the eigenvectors and eigenvalues in
   descending order by eigenvalues.
1. Pick the first $k$ principal components and project $X$ onto them.
  1. For visualization, $k$ is usually 2 or 3.
  1. For dimensionality reduction or feature extraction, $k$ should be high
     enough to explain a decent amount of variances in the data, e.g.
     $\frac{\sum_{i=1}^k \lambda_i}{\sum_{i=1}^r \lambda_i}$ is above a certain
     threshold.

## SVD

The SVD theorem says that any matrix $A \in \mathbb{R}^{m \times n}$ can be
decomposed into

$$
\begin{align}
\underset{m\times n}{A}  &= \underset{m \times r,}{U} \underset{r \times r,}{\Sigma} \underset{r \times n}{V^{T}}
\end{align}
$$

where

* $U$ consists of left singular vectors, i.e. orthogonal eigenvectors of $AA^T$.
* $\Sigma$ is a diagnoal matrix of singular values, i.e. the square root of
  non-zero eigenvalues of $A^TA$ or $AA^T$ in descending order (see Apendices
  for a proof that $AA^T$ and $A^TA$ have the same eigenvalues).
* $V$ consists of right singular vectors, i.e. orthogonal eigenvectors of
  $A^TA$.
* $r$ is the rank of $A$.

This version of SVD is called the reduced SVD because only non-zero eigenvalues
of $A^TA$ or $AA^T$ are considered, the full SVD would be

$$
\begin{align}
\underset{m\times n}{A}  &= \underset{m \times m,}{U_f} \underset{m \times n,}{\Sigma_f} \underset{n \times n}{V_f^{T}}
\end{align}
$$

where

* $U_f$ is $U$ padded with columns of orthonormal vectors from the nullspace of $A^T$.
* $\Sigma_f$ is $\Sigma$ padded with zeros along the diagnoal to form the shape
  of $m \times n$.
* $V_f$ is $V$ padded with columns of orthonormal vectors from the nullspace of
  $A$.

One benefit of full SVD is that $U_f$ and $V_f$ are *bona fide* orthogonal
matrices with both-sided inverses, while $U$ and $V$ from reduced SVD may only
have left inverse. Here, we mainly focus on the reduced SVD as the padded
columns and zeros in the full SVD do not bring additional information.

## Apply SVD to PCA

Given the data matrix $X$, and the sample covariance matrix is

$$
\begin{align}
S &= \frac{1}{n-1}\sum_{i=1}^n (\mathbf{x}^{(i)} - \bar{\mathbf{x}}) (\mathbf{x}^{(i)} - \bar{\mathbf{x}})^T
\end{align}
$$

If we define the standardized dataset $\tilde{X}$ as

$$
\begin{align}
\tilde{X}
&= X - \begin{bmatrix}
| & \cdots & | \\
\bar{\mathbf{x}} & \cdots & \bar{\mathbf{x}}\\
| & \cdots & |
\end{bmatrix}
= \begin{bmatrix}
| & \cdots & | \\
\mathbf{x}_1 - \bar{\mathbf{x}} & \cdots & \mathbf{x}_n - \bar{\mathbf{x}}\\
| & \cdots & |
\end{bmatrix}
\end{align}
$$

then the sample covariance matrix can be represented as

$$
\begin{align}
S
&= \frac{1}{n-1} \tilde{X} \tilde{X}^T \\
&= \left( \frac{1}{\sqrt{n - 1}}\tilde{X} \right) \left( \frac{1}{\sqrt{n - 1}} \tilde{X} \right)^T
\end{align}
$$

If we apply SVD to $\frac{1}{\sqrt{n-1}} \tilde{X}$,

$$
\frac{1}{\sqrt{n-1}} \tilde{X} = U \Sigma V^T
$$

By definition of SVD, $U$ and $\Sigma^2$ are the eigenvectors and eigenvalues of
$S = \left( \frac{1}{\sqrt{n - 1}}\tilde{X} \right) \left( \frac{1}{\sqrt{n -
1}} \tilde{X} \right)^T$, so $U$ consists of the principal components of of $X$,
and $\Sigma^2$ consists of the variances of the coordinates after $X$ is
projected onto $U$. The sum of the variances equals $\text{Tr}(S)$.

See
[PCA-implementation-and-demo.ipynb](https://github.com/zyxue/sutton-barto-rl-exercises/blob/master/stats/PCA-implementation-and-demo.ipynb)
for a demo.

## Appendices
### Show $\mathbf{u}^T \mathbf{x}$ is the coordinate when $\mathbf{x}$ projected on onto unit vector $\mathbf{u}$

Suppose the projected vector is $\alpha \mathbf{u}$, where $\alpha$ is a scalar,
then

$$
\begin{align}
\mathbf{u}^T(\mathbf{x} - \alpha\mathbf{u})
&= 0 \\
\alpha
&= \frac{\mathbf{u}^T \mathbf{x}}{\mathbf{u}^T \mathbf{u}}
\end{align}
$$

if $\mathbf{u}$ is a unit vector, then $\alpha = \mathbf{u}^T \mathbf{x}$, i.e.
the coordinate of $\mathbf{x}$ when projected onto $\mathbf{u}$.

### Show $\text{Var}(\mathbf{u}^T\mathbf{x}) = \mathbf{u}^T S \mathbf{u}$

Let

$$
\begin{align}
\bar{\mathbf{x}}
&= \frac{1}{n} \sum_{i=1}^n \mathbf{x}^{(i)}
\end{align}
$$

Then,

$$
\begin{align}
\text{Var}(\mathbf{u}^T X)
&= \frac{1}{n-1}\sum_{i=1}^n (\mathbf{u}^T \mathbf{x}^{(i)} - \mathbf{u}^T \bar{\mathbf{x}})^2  \label{eq:def_var} \\
&= \frac{1}{n-1}\sum_{i=1}^n \Big[ \mathbf{u}^T (\mathbf{x}^{(i)} - \bar{\mathbf{x}}) \Big]^2 \\
&= \frac{1}{n-1}\sum_{i=1}^n \mathbf{u}^T (\mathbf{x}^{(i)} - \bar{\mathbf{x}}) (\mathbf{x}^{(i)} - \bar{\mathbf{x}})^T \mathbf{u} \\
&= \mathbf{u}^T \left[ \frac{1}{n-1}\sum_{i=1}^n (\mathbf{x}^{(i)} - \bar{\mathbf{x}}) (\mathbf{x}^{(i)} - \bar{\mathbf{x}})^T \right ] \mathbf{u} \label{eq:def_covar} \\
&= \mathbf{u}^T S \mathbf{u} \\
\end{align}
$$

Note,

* Eq. \eqref{eq:def_var} is the definition of sample variance.
* The middle part of Eq. \eqref{eq:def_covar} is the definition of sample
  covariance matrix.

Such relationship also applies to population variance and population covariance
matrix as shown below. We'll use $\text{Var}_p$ and $S_p$ to indicate they're
population statistics instead of sample ones.

$$
\begin{align}
\text{Var}_p(\mathbf{u}^T X)
&= \mathbb{E}[(\mathbf{u}^T\mathbf{x} - \mathbb{E}[\mathbf{u}^T \mathbf{x}])^2] \\
&= \mathbb{E}[(\mathbf{u}^T \mathbf{x})^2] - \mathbb{E}[\mathbf{u}^T\mathbf{x}]^2 \\
&= \mathbb{E}[\mathbf{u}^T \mathbf{x} \mathbf{x}^T \mathbf{u}] - \mathbb{E}[\mathbf{u}^T\mathbf{x}]\mathbb{E}[\mathbf{x}^T \mathbf{u}] \\
&= \mathbf{u}^T \mathbb{E}[\mathbf{x} \mathbf{x}^T] \mathbf{u} - \mathbf{u}^T \mathbb{E}[\mathbf{x}] \mathbb{E}[\mathbf{x}^T] \mathbf{u} \\
&= \mathbf{u}^T \Big( \mathbb{E}[\mathbf{x} \mathbf{x}^T] - \mathbb{E}[\mathbf{x}] \mathbb{E}[\mathbf{x}^T] \Big) \mathbf{u} \\
&=\mathbf{u}^T\Big( \mathbb{E}[(\mathbf{x} - \mathbb{E}[\mathbf{x}]) (\mathbf{x} - \mathbb{E}[\mathbf{x}])^T \Big) \mathbf{u} \\
&=\mathbf{u}^T S_p \mathbf{u} \\
\end{align}
$$

That

$$
\begin{align}
\mathbb{E}[(\mathbf{x} - \mathbb{E}[\mathbf{x}]) (\mathbf{x} - \mathbb{E}[\mathbf{x}])^T] = \mathbb{E}[\mathbf{x} \mathbf{x}^T] - \mathbb{E}[\mathbf{x}] \mathbb{E}[\mathbf{x}^T]
\end{align}
$$

is the random vector equivalent of

$$
\mathbb{E}[(X - \mathbb{E}[X])^2] = \mathbb{E}[X^2] - \mathbb{E}[X]^2
$$

for the variance of a scalar random variable. A quick proof:

$$
\begin{align}
\mathbb{E}[(\mathbf{x} - \mathbb{E}[\mathbf{x}]) (\mathbf{x} - \mathbb{E}[\mathbf{x}])^T]
&= \mathbb{E}[\mathbf{x} \mathbf{x}^T - \mathbb{E}[\mathbf{x}] \mathbf{x}^T - \mathbf{x} \mathbb{E}[\mathbf{x}^T] + \mathbb{E}[\mathbf{x}] \mathbb{E}[\mathbf{x}^T]] \\
&= \mathbb{E}[\mathbf{x} \mathbf{x}^T ] - \mathbb{E}[\mathbf{x}]\mathbb{E}[\mathbf{x}^T] - \mathbb{E}[\mathbf{x}]\mathbb{E}[\mathbf{x}^T] + \mathbb{E}[\mathbf{x}]\mathbb{E}[\mathbf{x}^T] \\
&= \mathbb{E}[\mathbf{x} \mathbf{x}^T ] - \mathbb{E}[\mathbf{x}]\mathbb{E}[\mathbf{x}^T]
\end{align}
$$

### Show covariance matrix $S$ has the same rank as $X$


Suppose $X$ has rank $r$.  First, we standardize $X$ by removing the $\bar{X}$
to obtain $\tilde{X}$ (See Apply SVD to PCA section for their specific
definitions). Standardization won't change the rank because $\bar{\mathbf{x}}$
is in the column space of $X$.

Then, we show $S = \frac{1}{n} \tilde{X}\tilde{X}^T$ has the same rank as $X$.

Suppose $\mathbf{x}$ is in the left nullspace of $\tilde{X}$, then

$$
\begin{align}
\mathbf{x}^T \tilde{X} &= 0 \\
\mathbf{x}^T \tilde{X} \tilde{X}^T &= 0 \\
\end{align}
$$

so $\mathbf{x}$ is also in the nullspace of $S$, i.e. $\mathbf{X}$ and $S$ have
the same left nullspace, since they all have the same column space, and rank equals
the number of dimensions of the column space minus that of the left nullspace,
so $S$ and $X$ have the same rank.


Similar proof can also show $X^T X$ has the same rank as $X$. In summary, all of
$X^T X$, $X^T X$ and $X$ have the same rank.

### Show $AB$ and $BA$ have the same eigenvalues

Suppose $\mathbf{x}$ is an eigenvector and $\lambda$ is an eigenvalue of $A$,
then

$$
\begin{align}
ABx &= \lambda x \\
BA(Bx) &= \lambda Bx
\end{align}
$$

So $\lambda$ is also an eigenvalue of $BA$ with the corresponding eigenvector
becoming $Bx$.
