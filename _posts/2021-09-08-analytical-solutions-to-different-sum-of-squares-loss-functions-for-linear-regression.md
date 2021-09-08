---
layout: post
title: Analytical solutions to different sum-of-squares loss functions for linear regression
author: Zhuyi Xue
tags: statistics
---

# Introduction

The linear model:

$$
\begin{align}
X \hat{\mathbf{w}} &= \hat{\mathbf{y}}
\end{align}
$$

where 

* $X$ is a $N \times D$ data matrix, $N$ is the number of data points, and $D$ is the number of feature dimensions.
*$\hat{\mathbf{w}}$ is the coefficients to be esitmated, and
* $\hat{y}$ is the predicted targets.

Suppose the ground truth targets are represented by $\mathbf{y}$.

# Regular sum-of-squares loss


$$
\begin{align}
L
&= 
\frac{1}{2} \left(\hat{\mathbf{y}} - \mathbf{y} \right)^T \left(\hat{\mathbf{y}} - \mathbf{y} \right) \\
&= 
\frac{1}{2} \left(X\hat{\mathbf{w}} - \mathbf{y} \right)^T \left(X\hat{\mathbf{w}} - \mathbf{y} \right)
\end{align}
$$

Take derivative of $L$ and set it to zero,

$$
\begin{align}
\frac{\partial L}{\partial \mathbf{w}}
&= 
X^T \left(X\hat{\mathbf{w}} - \mathbf{y} \right) = 0 \\
X^TX\hat{\mathbf{w}}
&= X^T \mathbf{y} \\
\hat{\mathbf{w}}
&= (X^TX)^{-1}X^T \mathbf{y}
\end{align}
$$

So the predictions are 

$$
\begin{align}
\hat{\mathbf{t}} &= X(X^TX)^{-1}X^T \mathbf{y}
\end{align}
$$

# Weighted sum-of-squares loss

$$
\begin{align}
L
&= 
\frac{1}{2} \left(\hat{\mathbf{y}} - \mathbf{y} \right)^T R \left(\hat{\mathbf{y}} - \mathbf{y} \right) \\
&= 
\frac{1}{2} \left(X\hat{\mathbf{w}} - \mathbf{y} \right)^T R \left(X\hat{\mathbf{w}} - \mathbf{y} \right)
\end{align}
$$

where $R$ is a diagonal weight matrix for each data point, $R$ is in the form of 
$$
\begin{bmatrix}
r_1 &  & \\ 
 & \ddots & \\ 
 &  & r_n
\end{bmatrix}
$$.


Similar to the case of regular sum-of-squares loss, take derivative of $L$ and set it to zero,

$$
\begin{align}
\frac{\partial L}{\partial \mathbf{w}}
&= 
X^T R \left(X\hat{\mathbf{w}} - \mathbf{y} \right) = 0 \\
X^T R X\hat{\mathbf{w}}
&= X^T R \mathbf{y} \\
\hat{\mathbf{w}}
&= (X^T R X)^{-1}X^T R \mathbf{y}
\end{align}
$$

and the predictions are 

$$
\begin{align}
\hat{\mathbf{t}} &= X(X^T R X)^{-1}X^T R \mathbf{y}
\end{align}
$$
