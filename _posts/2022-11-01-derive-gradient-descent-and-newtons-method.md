---
layout: post
title: Derive gradient descent and Newton's method
author: Zhuyi Xue
tags: statistics
---

Recall a smooth function $f(\mathbf{x})$ can be approximated at
$\mathbf{x} = \mathbf{a}$ with the Taylor series,


$$
\begin{equation}
f(\mathbf{x})
\approx f(\mathbf{a}) + \nabla f(\mathbf{\mathbf{a}})^T (\mathbf{x} - \mathbf{a}) + \frac{1}{2!} (\mathbf{x} - \mathbf{a})^T \nabla^2 f(\mathbf{a}) (\mathbf{x} - \mathbf{a})  + \cdots \\
\end{equation}
$$

Consider now we would like to minimize $f(\mathbf{x})$, and derive two methods
for doing so.

# Gradient descent

Consider when $\mathbf{x}$ is so close to $\mathbf{a}$ that the 2nd-order term
becomes negligible compared to the first-order term. Given $f(\mathbf{a})$ is a
constant, then we can focus on minimizing the first-order term, i.e.

$$
\begin{align}
\ell_{gd} &= \nabla f(\mathbf{a})^T(\mathbf{x} - \mathbf{a})
\end{align}
$$

where $gd$ in the subscript means gradient descent. 

One may be tempted to try to calculate
$\frac{\partial{\ell_{gd}}}{\partial{\mathbf{x}}}$ and set it to zero. But
$\ell_{gd}$ is a linear function of $\mathbf{x}$ without any critical point
(e.g. minimum, maximum or saddle point) unless $\nabla f(\mathbf{a}) =
0$. Instead, let's think *in which direction should we move $\mathbf{x}$ away
from $\mathbf{a}$ so that $\ell_{gd}$ would decrease the fastest*?

Note, $\ell_{gd}$ can be rewrritten as

$$
\begin{align}
\ell_{gd}
&= \nabla f(\mathbf{a})^T r \mathbf{u}_x \label{eq:convert_to_vec_multiplication} \\ 
&= r \left|\nabla f(\mathbf{a})\right| \cos \theta \label{eq:convert_to_theta_optimization} \\ 
\end{align}
$$

where 

* In Eq. \eqref{eq:convert_to_vec_multiplication}, $\mathbf{x} - \mathbf{a}$ is
  factored into two parts: a positive scalar $r$ and a unit vector
  $$\mathbf{u}_x$$.
* In Eq. \eqref{eq:convert_to_theta_optimization}, $\theta$ is the angle
  between $\nabla f(\mathbf{a})$ and $\mathbf{u}_x$.

We fix *$r$ to be a constant*. In other words, we're only considering moving
$\mathbf{x}$ away from $\mathbf{a}$ in a fixed distance towards some direction
determined by $\theta$. Note, $r$ is very small as we're only considering the
vicinity of $\mathbf{a}$ where the second-order and up terms of the Taylor
series can be ignored. Apparently, $\ell_{gd}$ is minimized when $\cos \theta =
-1$, i.e. $\theta = \pi$, which means $\mathbf{u}_x$ is in the exact opposite
direction of $\nabla f(\mathbf{a})$.

$$
\begin{align}
\min_{\theta} \ell_{gd}
&= - r \left|\nabla f(\mathbf{a})\right| \\ 
\end{align}
$$

Therefore, to decrease $f(\mathbf{x})$ the quickest by moving $\mathbf{x}$ in a
fixed distance $r$, we should move $\mathbf{x}$ in the direction of $\nabla
f(\mathbf{a})$. The update can be written as

$$
\begin{align}
\mathbf{x} - \mathbf{a} &= - r \nabla f(\mathbf{a}) \\
\mathbf{x} &= \mathbf{a} - r \nabla f(\mathbf{a}) \label{eq:gradient_descent_update}
\end{align}
$$

The method of updating $\mathbf{x}$ following
Eq. \eqref{eq:gradient_descent_update} is known as the **gradient descent**.

Because $\nabla f(\mathbf{a}$) is the direction that makes $f(\mathbf{x})$
descent the quickest, gradient descent is also known as **steepest descent**.

Analogously, if $\theta$ is set to $0$, i.e. moving $\mathbf{x}$ along the
direction of $\nabla f(\mathbf{x})$ instead of $- \nabla f(\mathbf{x})$, the
update rule becomes $$ \mathbf{x} = \mathbf{a} + \alpha \nabla f(\mathbf{a})
\label{eq:gradient_ascent_update}$$, and we are maximizing
$f(\mathbf{x})$. Such method is known as gradient ascent or steepest ascent.

## Newton's method

Gradient descent used only up to the first-order term of the Taylor series. Can
we use information from the the second-order term for minimizing
$f(\mathbf{x})$?

Consider when $\mathbf{x}$ is a bit further but still very close to
$\mathbf{a}$ that all the 3rd and up terms are negligible, so we can focus on
minimizing

$$
\begin{align}
\ell_{newton}
&= \nabla f(\mathbf{\mathbf{a}})^T (\mathbf{x} - \mathbf{a}) + \frac{1}{2} (\mathbf{x} - \mathbf{a})^T \nabla^2 f(\mathbf{a}) (\mathbf{x} - \mathbf{a})   \\
\end{align}
$$

Note, $\ell_{newton}$ is a quadratic function of $\mathbf{x}$, so we can
calculate its derivative and set it to zero for minimization, assuming the
$\nabla^2 f(\mathbf{a}) \succeq 0$ (positive semi-definite).

$$
\begin{align}
\frac{\partial \ell_{netwon}}{\partial \mathbf{x}}
&= \nabla f(\mathbf{\mathbf{a}}) + \nabla^2 f(\mathbf{a}) (\mathbf{x} - \mathbf{a}) \\
&= \nabla f(\mathbf{\mathbf{a}}) + \nabla^2 f(\mathbf{a}) \mathbf{x} - \nabla^2 f(\mathbf{a}) \mathbf{a} \\ 
&= \nabla f(\mathbf{\mathbf{a}}) + H \mathbf{x} - H \mathbf{a} \label{eq:replace_with_H} \\ 
&= 0 \\
H \mathbf{x} &= H \mathbf{a} - \nabla f(\mathbf{\mathbf{a}}) \\
\mathbf{x} &= \mathbf{a} - H^{-1} \nabla f(\mathbf{\mathbf{a}}) \label{eq:newton_method_update} \\
\end{align}
$$

In Eq. \eqref{eq:replace_with_H}, we used $H = \nabla^2 f(\mathbf{a})$ for
clarity.

The method of updating $\mathbf{x}$ following
Eq. \eqref{eq:newton_method_update} is known as the Newton's method.

## Comparison

Note, comparing the update rules of gradient descent and Newton's method,
Eq. \eqref{eq:gradient_descent_update} and Eq. \eqref{eq:newton_method_update}
look quite similar except the key difference that $\alpha$ (a scalar) is
replaced with $H^{-1}$ (a matrix).

Note, the ways we pick the next $\mathbf{x}$ in the two methods are
fundamentally different. 

* In gradient descent, we explicitly choose $\mathbf{u}_x$ to be in the
  opposite direction of $\nabla f(\mathbf{a})$ (i.e. $\cos \theta = -1$) so
  that $\mathbf{x}$ is always moving in the direction of decreasing
  $f(\mathbf{x})$. If a minimum doesn't exist, gradient descent would move
  $f(\mathbf{x})$ towards $-\infty$.
* In Newton's method, since the direction is determined by $H^{-1} \nabla
  f(\mathbf{a})$, it may be in the direction of increasing $f(\mathbf{x})$ if
  $f(\mathbf{x})$ is concave around $\mathbf{a}$. We can check the convexity of
  $f(\mathbf{x})$ around $\mathbf{a}$ by inspecting the definiteness of $H$
  (TODO: write up how such test works):

  * if $H \succeq 0$, i.e. all eigenvalues $\ge 0 $, then $\ell_{newton}$
    has a minimum. 
	  * The insight is that when $f(\mathbf{x})$ is written using the
		eigenvectors of $H$ as the basis, denote it as $f(\mathbf{x}')$, there
		would be no cross-terms of components of $\mathbf{x}'$, and the
		eigenvalues of $H$ are the second-order partial derivatives of
		$f(\mathbf{x}')$ along the eigenvectors, which means along all
		directions in the space of $\mathbf{x}$, $f$ would be curving up, hence
		convex.
  * if $H \preceq 0$, i.e. all eigenvalues $\le 0$, $\ell_{newton}$ is concave.
  * Otherwise, then $\ell_{newton}$ has a saddle point.

# References

* Nice visualization of gradient descent and Newton's method in action:
  [https://jermwatt.github.io/machine_learning_refined/notes/4_Second_order_methods/4_4_Newtons.html](https://jermwatt.github.io/machine_learning_refined/notes/4_Second_order_methods/4_4_Newtons.html).
