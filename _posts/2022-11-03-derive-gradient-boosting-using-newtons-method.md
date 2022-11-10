---
layout: post
title: Dervie gradient boosting using Newton's method
author: Zhuyi Xue
tags: statistics
---

* toc
{:toc}

This post rederives the gradient boosting algorithm from the
[XGBoost paper](https://arxiv.org/pdf/1603.02754.pdf) and clarifies what each
tree is fitted to predict.

# Second-order Taylor approximation of loss function

For gradient boosting, at step $t$, the loss function can be written as

$$
\begin{align}
\mathcal{L}^{(t)}
&= \sum_{i=1}^n l\left(y_i, \hat y_i^{(t - 1)} + f_t(\mathbf{x}_i)\right) + \Omega(f_t) \label{eq:xgboost_loss} \\
&= \sum_{i=1}^n l\left(y_i, \hat y_i^{(t)} \right) + \Omega(f_t) \label{eq:loss_in_hat_y_t}
\end{align}
$$

Note,

* in Eq. \eqref{eq:xgboost_loss}, we used the XGBoost notation, where
	* $\mathbf{x}$ is the feature vector of example $i$.
	* $y_i$ is the label of example $i$.
	* $\hat{y}_i^{(t - 1)}$ is the prediction from $t - 1$ already trained trees.
	* $l$ is the loss function for a single example.
	* $f_t$ is the $t$-th tree to be trained.
	* $\Omega$ is the regularization term for $f_t$.
* in Eq. \eqref{eq:loss_in_hat_y_t}, we set $\hat{y}_i^{(t)} = \hat{y}_i^{(t - 1)} + f_t(\mathbf{x}_i)$ Note that at step $t$,
	* $\hat{y}_i^{(t-1)}$ is a constant,
	* $f_t(\mathbf{x}_i)$ is a variable,
	* so $\hat{y}_i^{(t)}$ is also a varaible.

To avoid clutter, we

* drop $t$ from $\mathcal{L}^{(t)}$ and $f_t$ as we only foucs on optimization
  at step $t$ in this post,
* ignore the regularization terms, and
* rewrite $l\left(y_i, \hat y_i^{(t)} \right)$ as $l_i$, and $f_t(\mathbf{x}_i)$ as $f_i$.

So the loss function can be rewrriten as

$$
\begin{align}
\mathcal{L}
&= \sum_{i=1}^n l_i = \mathbb{1}^T \begin{bmatrix}
l_1 \\
\vdots \\
l_n
\end{bmatrix} \\
\end{align}
$$

To prepare for Taylor approximation, denote

* $\mathbf{f} = \begin{bmatrix} f_1, \cdots, f_n \end{bmatrix}^T$, and
* $\hat{\mathbf{y}}^{(t)} = \begin{bmatrix} \hat{y}_1^{(t)}, \cdots, \hat{y}_n^{(t)} \end{bmatrix}^T$

the first-order derivative wrt. $\hat{\mathbf{y}}$ is

$$
\begin{align}
\mathbf{g}
= \frac{\partial \mathcal{L}}{\partial \hat{\mathbf{y}}^{(t)}}
= \begin{bmatrix}
\frac{\partial l_i}{\partial \hat{y}_1^{(t)}}\\
\vdots \\
\frac{\partial l_i}{\partial \hat{y}_n^{(t)}}\\
\end{bmatrix}
= \begin{bmatrix}
\frac{\partial l_i}{\partial f_1}\\
\vdots \\
\frac{\partial l_i}{\partial f_n}\\
\end{bmatrix}
= \frac{\partial \mathcal{L}}{\partial \mathbf{f}}
\end{align}
$$

and the second-order derivative, aka. Hessian,

$$
\begin{align}
H
= \frac{\partial^2 \mathcal{L}}{\partial \left(\hat{\mathbf{y}}^{(t)} \right)^2}
= \begin{bmatrix}
\frac{\partial^2 l_i}{\partial \left(\hat{y}_1^2 \right)^2} & & \\
& \ddots & \\
& & \frac{\partial^2 l_i}{\partial \left(\hat{y}_n^2 \right)^2}\\
\end{bmatrix}
= \begin{bmatrix}
\frac{\partial^2 l_i}{\partial f_1^2} & & \\
& \ddots & \\
& & \frac{\partial^2 l_i}{\partial f_n^2}\\
\end{bmatrix}
= \frac{\partial^2 \mathcal{L}}{\partial \mathbf{f}^2}
\end{align}
$$

The reason to $\frac{\partial l_i}{\partial \hat{y}_i^{(t)}} = \frac{\partial
l_i}{\partial f_i}$ and $\frac{\partial^2 l_i}{\partial \left(\hat{y}_i^{(t)}
\right)^2} = \frac{\partial^2 l_i}{\partial f_i^2}$ is because $\hat{y}_i^{(t)}
= \hat{y}^{(t - 1)} + f_i$ and $\hat{y}^{(t - 1)}$ is a constant. By chain
rule,

$$
\frac{\partial l_i}{\partial f_i}
= \frac{\partial l_i}{\partial \hat{y}_i^{(t)}} \frac{\partial \hat{y}_i^{(t)}}{\partial f_i}
= \frac{\partial l_i}{\partial \hat{y}_i^{(t)}} \frac{\partial \left( \hat{y}_i^{(t - 1)} + f_i\right )}{\partial f_i}
= \frac{\partial l_i}{\partial \hat{y}_i^{(t)}} \cdot 1
= \frac{\partial l_i}{\partial \hat{y}_i^{(t)}}
$$

Then, the second-order Taylor approximation at $\hat{\mathbf{y}}^{(t-1)}$,
i.e. when $\mathbf{f} = \mathbf{0}$, is

$$
\begin{align}
\mathcal{L}
\approx \tilde{\mathcal{L}}
&= \mathbb{1}^T \begin{bmatrix}
l\left(y_1, \hat{y}_i^{(t - 1)}\right) \\
\vdots \\
l\left(y_n, \hat{y}_n^{(t - 1)}\right) \\
\end{bmatrix} + \left(\hat{\mathbf{y}}^{(t)} - \hat{\mathbf{y}}^{(t - 1)} \right)^T \mathbf{g} + \left(\hat{\mathbf{y}}^{(t)} - \hat{\mathbf{y}}^{(t - 1)} \right)^T H \left(\hat{\mathbf{y}}^{(t)} - \hat{\mathbf{y}}^{(t - 1)} \right) \\
&\sim \mathbf{f}^T \mathbf{g} + \mathbf{f}^T H \mathbf{f} \\
&= \sum_{i=1}^n g_i f_i + \sum_{i=1}^n \frac{1}{2} h_i f_i^2 \\
&= \sum_{i=1}^n g_i f_i + \frac{1}{2} h_i f_i^2 \label{eq:2nd_order_approx_final} \\
\end{align}
$$

Note

* $\approx$ means approximation.
* $\sim$ means simplification with constant or scalar removed, which does not affect minimization of the loss function.
* $g_i$ is the $i$-th element of the gradient vector $\mathbf{g}$.
* $h_i$ is the $i$-th element of the diagonal Hessian matrix, i.e. $h_i =
  H_{i,i} = \frac{\partial^2 l_i}{\partial f_i^2}$.
* Both $g_i$ and $h_i$ are specific the step $t$.

Note Eq. \eqref{eq:2nd_order_approx_final} matches Eq. (3), in the XGBoost
paper.

# Minimizing the $\tilde{\mathcal{L}}$ is equivalent to fitting a tree to predict $- \frac{g_i}{h_i}$

To minimize $\tilde{\mathcal{L}}$, we could calculate $\frac{\partial
\tilde{\mathcal{L}}}{\partial \mathbf{f}}$ and set it to zero, which would lead
to setting

$$
\begin{align}
\mathbf{f} = - H^{-1} \mathbf{g} = \begin{bmatrix}
- \frac{g_1}{h_1} \\
\vdots \\
- \frac{g_n}{h_n}
\end{bmatrix}
\end{align}
$$

which is exactly the [Newton's
method](https://en.wikipedia.org/wiki/Newton%27s_method). However, note $f$ is a
function and needs to be generalizable to unknown examples, so instead of simply
creating a mapping between $f_i$ and $- \frac{g_i}{h_i}$, we need to train $f_i$
properly to ensure its generalizability. This is a typical machine learning
task, and $\mathbf{f} = - H^{-1} \mathbf{g}$ form the training set.

We show that minimizing $\tilde{\mathcal{L}}$ is equivalent to fitting $f$ to
predict $- \frac{g}{h}$. Ignoring the scalar and constant terms that don't
affect minimization, $\tilde{\mathcal{L}}$ can be rewritten as

$$
\begin{align}
\tilde{\mathcal{L}}
&\sim \sum_{i=1}^n g_i f_i + \frac{1}{2} h_i f_i^2 \label{eq:only_g_and_h_terms} \\
&\sim \sum_{i=1}^n h_i \left( f_i + \frac{g_i}{h_i} \right )^2  \\
&\sim \sum_{i=1}^n h_i \left( f_i - \left( - \frac{g_i}{h_i} \right ) \right )^2
\end{align}
$$

Therefore, $\tilde{\mathcal{L}}$ is a weighted mean squared error (MSE) loss
function of the ground truth $- \frac{g_i}{h_i}$ and the prediction $f_i$. The
MSE is weighted by $h_i$. The intuition for such weighting can be understood as
follows. Consider for two examples $i$ and $j$, $h_i > h_j$, when $f_i +
\frac{g_i}{h_i} = f_j + \frac{g_j}{h_j}$, then $l(f_i) > l(f_j)$, hence we want
to penalize $f_i + \frac{g_i}{h_i}$ more heavily.

It's interesting to note that in gradient boosting, it has many nested machine
learning tasks. Each decision tree is fitted to predict $- \frac{g_i}{h_i}$ at
each step $t$, while those trees together predict the label $y_i$ as governed by
the property of a loss function, e.g. when $\mathcal{L}$ is of the form of MSE,
the ensemble predicts conditional mean. Properties of some common loss functions
are analyzed in
[here](https://github.com/zyxue/sutton-barto-rl-exercises/tree/b0bdf5f9a6e54f76d65a04d69382809d262287c9/supervised/loss_function_properties).

One more note on convexity of the loss function. Recall when optimizing a
function using Newton's method, $\tilde{\mathcal{L}}$ has to be convex, i.e. $H$
needs to be positive semi-definite, for it to have a minimum. Given $H$ is
diagonal in our case, so its diagonal elements are also eigenvalues of $H$, and
a positive semi-definite $H$ means $\frac{\partial^2 l(f_i)}{\partial f_i^2} \ge
0$ for all $i$. Let's exam a few common loss function to get a more concrete
sense of what this looks like.

### Logloss

Suppose $\hat{y}_i^{(t)}$ is the predicted
[logit](https://en.wikipedia.org/wiki/Logit) (aka. log-odds), which can be
converted to probability via

$$
\begin{align}
p_i^{(t)} = \frac{e^{\hat{y}_i^{(t)}}}{1 + e^{\hat{y}_i^{(t)}}}
\end{align}
$$

Hence, the logloss can be written as

$$
\begin{align}
l_i
&= - \Big[ y_i \ln p_i^{(t)} + (1 - y_i ) \ln (1 - p_i^{(t)}) \Big ] \\
&= - \Big[ y_i (\hat{y}_i^{(t)} - \ln (1 + e^{\hat{y}_i^{(t)}})) - (1 - y_i) \ln (1 + e^{\hat{y}_i^{(t)}}) \Big] \\
&= - \Big[ y_i\hat{y}_i^{(t)} - y_i \ln (1 + e^{\hat{y}_i^{(t)}}) - \ln (1 + e^{\hat{y}_i^{(t)}}) + y_i \ln (1 + e^{\hat{y}_i^{(t)}}) \Big] \\
&= - \Big[ y_i\hat{y}_i^{(t)} - \ln (1 + e^{\hat{y}_i^{(t)}}) \Big] \\
&= \ln (1 + e^{\hat{y}_i^{(t)}}) - y_i\hat{y}_i^{(t)}
\end{align}
$$

Then,

$$
\begin{align}
g_i
= \frac{\partial l_i}{\partial \hat{y}_i^{(t)}}
= \frac{e^{\hat{y}_i^{(t)}}}{1 + e^{\hat{y}_i^{(t)}}} - y_i
= p_i^{(t)} - y_i
\end{align}
$$

and

$$
\begin{align}
h_i
= \frac{\partial g_i}{\partial \hat{y}_i^{(t)}}
= \frac{\left(1 + e^{\hat{y}_i^{(t)}} \right )  e^{\hat{y}_i^{(t)}} - e^{\hat{y}_i^{(t)}} e^{\hat{y}_i^{(t)}}  }{\left( 1 + e^{\hat{y}_i^{(t)}} \right )^2}
= \frac{e^{\hat{y}_i^{(t)}}}{1 + e^{\hat{y}_i^{(t)}}} \frac{1}{1 + e^{\hat{y}_i^{(t)}}}
= p_i^{(t)} ( 1 - p_i^{(t)}) > 0
\end{align}
$$

Therefore, logloss is convex.

### Mean squared error (MSE)

The MSE is written as

$$
\begin{align}
l_i
&= \frac{1}{2} \left( \hat{y}_i^{(t)} - y_i \right)^2 \\
\end{align}
$$

Then,

$$
\begin{align}
g_i = \frac{\partial l_i}{\partial \hat{y}_i^{(t)}}
&= \hat{y}_i^{(t)} - y_i  \\
\end{align}
$$

and

$$
\begin{align}
h_i
= \frac{\partial g_i}{\partial \left( \hat{y}_i^{(t)} \right)^2} = 1 > 0
\end{align}
$$

So MSE is also convex in $\hat{y}_i^{(t)}$.

Typically, it only makes sense for a function with positive semi-definite
Hessian in $\hat{\mathbf{y}}^{(t)}$ to be a loss function.

# Growing a tree to minimize $\tilde{\mathcal{L}}$

To be consistent with XGBoost notation, let's denote the structure of a
decision tree as $q$, the number of leaves as $T$, and the leaf weights as
$\mathbf{w}$. Apparently, once $q$ is fixed, so is $T$. Let's denote such
relationship as $T = z(q)$. A decision tree is fully characterized by $q$ and
$\mathbf{w}$. Then Eq. \eqref{eq:only_g_and_h_terms} can be written as

$$
\begin{align}
\tilde{\mathcal{L}} (\mathbf{f})
&= \sum_{i=1}^n \left[ g_i f_i + \frac{1}{2} h_i f_i^2 \right ] \\
&= \sum_{j=1}^{z(q)} \left[ \sum_{i \in I_j} g_i w_j + \frac{1}{2} h_i w_j^2 \right ] \\
&= \tilde{\mathcal{L}} (q, \mathbf{w}) \\
\end{align}
$$

where

* $w_j$ is the weight of the $j$th leaf.
* $I_j$ is set of examples that fall into the $j$th leaf.

Basically, we rewrite $\tilde{\mathcal{L}}$ from a function of tree outputs
$\mathbf{f}$ to a function of the tree parameters $q$ and $\mathbf{w}$.

Therefore,

$$
\begin{align}
\min_{\mathbf{f}} \tilde{\mathcal{L}} (\mathbf{f})
&= \min_{q, \mathbf{w}} \tilde{\mathcal{L}}(q, \mathbf{w}) \\
\end{align}
$$

In general, it is difficult to optimize for both $q$ and $\mathbf{w}$
simultaneously. Instead, we can optimize for $q$ and $\mathbf{w}$ separately in
an iterative fashion.

$$
\min_q \min_{\mathbf{w}} \tilde{\mathcal{L}}(q, \mathbf{w})
$$


### Minimize over $\mathbf{w}$

Consider a given $q$, so $T$ is fixed. Set $G_j = \sum_{i \in I_j} g_i$ and
$H_j = \sum_{i \in I_j} h_i$. Then

$$
\begin{align}
\tilde{\mathcal{L}}(\mathbf{w})
&= \sum_{j=1}^T \left[ G_j w_j + \frac{1}{2} H_j w_j^2 \right ] \\
&= \sum_{j=1}^T \frac{1}{2} H_j \left[ w_j + \frac{G_j}{H_j} \right ]^2 - \frac{1}{2} \frac{G_j^2}{H_j}
\end{align}
$$

Hence,

$$
\begin{align}
\arg \min_{\mathbf{w}} \tilde{\mathcal{L}}(\mathbf{w}) = \mathbf{w}^*
&= \begin{bmatrix}
-\frac{G_1}{H_1} \\
\vdots \\
-\frac{G_T}{H_T}
\end{bmatrix} \\
\end{align}
$$

and

$$
\begin{align}
\min_{\mathbf{w}} \tilde{\mathcal{L}}(\mathbf{w}) = \tilde{\mathcal{L}}^*(q) =
&= - \frac{1}{2} \sum_{j=1}^T \frac{G_j^2}{H_j} \\
\end{align}
$$

### Minimize over $q$

Now we can calculate the minimal loss for a given $q$, then for a given split
point $p$, denote the tree structure after the split as $q'$ with $T'$ leaves,
we can calculate the reduction in loss due to the split.

$$
\begin{align}
\Delta_p = \tilde{\mathcal{L}}^* (q) - \tilde{\mathcal{L}}^* (q')
&= -\frac{1}{2} \sum_{j=1}^T \frac{G_j^2}{H_j} + \frac{1}{2} \sum_{j=1}^{T'} \frac{G_j^2}{H_j} \\
&\sim \sum_{j=1}^{T'} \frac{G_j^2}{H_j} - \sum_{j=1}^{T} \frac{G_j^2}{H_j} \\
\end{align}
$$


Note, all terms in $T'$ and $T$ cancel except for those related to the one leaf
being split. Denote this leaf as $S$ and the new leaves result from the split
as $L$ and $R$. $\Delta_p$ can be simplified to

$$
\begin{align}
\Delta_p
&= \frac{G_L^2}{H_L} + \frac{G_R^2}{H_R} - \frac{G_S^2}{H_S} \label{eq:delta_by_LR} \\
\end{align}
$$

Note, Eq. \eqref{eq:delta_by_LR} matches Eq. (7) from the XGBoost paper.

Therefore, to grow the tree by one split, we can evaluate all split points with
Eq. \eqref{eq:delta_by_LR}, and select the one that results in max loss
reduction, i.e.

$$
\begin{align}
\min_q \tilde{L}(q) = \max_p \Delta_p
\end{align}
$$


# Reference

* [XGBoost Section 2.2](https://arxiv.org/pdf/1603.02754.pdf).
* Derivation of $g_i$ and $h_i$ for loss functions with simplified notations:
  [derive-gradient-and-hessian-of-cross-entropy-and-MSE.ipynb](https://github.com/zyxue/sutton-barto-rl-exercises/blob/b0bdf5f9a6e54f76d65a04d69382809d262287c9/supervised/derive-gradient-and-hessian-of-cross-entropy-and-MSE.ipynb).