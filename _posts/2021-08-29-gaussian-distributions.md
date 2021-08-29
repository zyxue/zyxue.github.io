---
layout: post
title: Gaussian distributions
author: Zhuyi Xue
tags: statistics
---

Put univariate and multivariate Gaussians together for comparison:

Univariate Gaussian: 

$$
\begin{align}
p(x) 
&= \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left \{- \frac{1}{2} \frac{(x - \mu)^2}{\sigma^2} \right \} \\
&= \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left \{- \frac{1}{2} (x - \mu)\frac{1}{\sigma^2}(x - \mu) \right \} \\
\end{align}
$$


Multivariate Gaussian:

$$
\begin{equation}
p(\mathbf{x}) = \frac{1}{\sqrt{(2 \pi)^D |\Sigma|}} \exp \left \{\frac{1}{2} (\mathbf{x} - \boldsymbol{\mu})^T \Sigma^{-1} (\mathbf{x} - \boldsymbol{\mu}) \right \}
\end{equation}
$$
