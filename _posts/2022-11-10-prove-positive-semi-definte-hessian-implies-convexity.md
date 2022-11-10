---
layout: post
title: Prove positive semi-definite Hessian implies convexity
author: Zhuyi Xue
tags: statistics
---

Suppose a function $f(\mathbf{x})$ is smooth around a point ($\mathbf{a}$),
then $f(\mathbf{x})$ can be approximated around this point with 2nd-order
Taylor series, which is a quadratic function of the form:

$$
\begin{align}
f(\mathbf{x})
\approx \tilde{f}(\mathbf{x})
= \frac{1}{2} \mathbf{x}^T H \mathbf{x} + \mathbf{b}^T \mathbf{x} + c \\
\end{align}
$$

where

* $H = \frac{\partial^2 f(\mathbf{x})}{\partial \mathbf{x}^2}\Big\|_{\mathbf{x} = \mathbf{a}}$, i.e. the Hessain at $\mathbf{a}$.
* $c$ is a constant.

Note, $H$ is symmetric, so it can be eigen-decomposed with real eigenvalues $\Lambda$ and orthogonal eigenvectors $Q$, so $H = Q \Lambda Q^T$.

Then,

$$
\begin{align}
\tilde{f}(\mathbf{x})
&= \frac{1}{2} \mathbf{x}^T Q \Lambda Q^T \mathbf{x} + \mathbf{b}^T \mathbf{x} + c \\
&= \frac{1}{2} \mathbf{y}^T \Lambda \mathbf{y} + \mathbf{b}^T Q \mathbf{y} + c \\
&= \frac{1}{2} \sum_{i=1}^n \lambda_i y_i^2 + \mathbf{b}^T \mathbf{q}_i y_i + c
\end{align}
$$

Note, in the second equality, we set $\mathbf{y} = Q^T \mathbf{x}$, which
effectively changed the basis from $\mathbf{I}$ to $Q$. As seen, there is no
cross-term among $y_i$s. If $H$ is positive semi-definite, then $\lambda_i \ge
1$, so the quadratic function $\tilde{f}(\mathbf{x})$ is curved up in all
directions as defined by $Q$.


# References

* Related StackOverflow posts:
  * [https://math.stackexchange.com/questions/4150646/why-positive-definite-hessian-implies-convexity](https://math.stackexchange.com/questions/4150646/why-positive-definite-hessian-implies-convexity)
  * [https://math.stackexchange.com/questions/2083629/why-does-positive-semi-definiteness-imply-convexity](https://math.stackexchange.com/questions/2083629/why-does-positive-semi-definiteness-imply-convexity)
  * [https://math.stackexchange.com/questions/1599846/hessian-matrix-for-convexity-of-multidimensional-function](https://math.stackexchange.com/questions/1599846/hessian-matrix-for-convexity-of-multidimensional-function)
