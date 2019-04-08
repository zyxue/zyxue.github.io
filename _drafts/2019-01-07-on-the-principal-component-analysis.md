---
layout: post
title:  On the principal component analysis
author: Zhuyi Xue
tags: linear algebra
---

**Outline**

1. PCA perspectives: minimize MSE & maximizing variance along the projected direction)
1. Use SVD to derive PCA
1. reduced SVD and truncated SVD (show an exact example from MIT video maybe)
1. what's randomized SVD

In this post, I will summarize my knowledge about principal component analysis
(PCA) and its relationship to singular value decomposition (SVD) from first
principal.

# Intuition about PCA

Consider that we collected a set of data points $\\{x^{(1)}, \cdots,
x^{(m)}\\}$, where $x^{(i)} \in \mathbb{R}^n$. Putting all data together, we
form a matrix

$$X = \Big[\big(x^{(1)}\big)^T, \cdots, \big(x^{(m)}\big)^T \Big], \;\;\; X \in \mathbb{R}^{m \times n}$$

The data here is in $n$ dimension, **what PCA does is essentially transform the
data into a lower dimension $d$ ($d \le n$)**, while keeping certain properties
of the data so that it is still representative of the data in the
high-dimension space. Often, I find it helpful to illustrate this idea of
dimensionality reduction in my mind with an example transformation from 2d data
to 1 data or 3d to 2d.

In general, data in lower dimension is less noisy, and hence easier to
manipulate. When the dimension is reduced to lower than three dimensions, it
also becomes convenient to visualize the data.

From an intuitive perspective, we would like to achieve two goals w.s.t. the two
properties of the data:

* variance within the data, which we would like to maintain during transformation
* difference between the high- and low-demension spaces, which we would like to reduce

First, we would like to keep the variance which is essentially the information
contained in the data. I magine a situation where 2d data on a plane are
projected (a different name for transformed) onto a line in the 1d space.
Suppose the two dimensions are high correlated, if the projection vector is
poorly selected and the projected data is high concentrated around a single
point, then we lost most information in data and it won't be representative of
the data in the 2d space anymore.

Second, the difference between data in the low ($d$) and high ($n$) dimensions
should be kept to a minimum level. the smaller the difference, the more
represetative the data is after the transformation.

As we will see, when we formulate the difference properly with mean squared
error (MSE), the above two objectives lead to the same solution, that is PCA.

### Maximize the variance

Watch https://www.youtube.com/watch?v=L-pQtGm3VS8

### Minimize the difference

# Singular Value Decomposition

### reduced SVD


### truncated SVD

Use SVD to solve PCA

# Further note

Randomized SVD
