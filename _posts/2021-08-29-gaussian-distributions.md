---
layout: post
title: Gaussian distributions
author: Zhuyi Xue
tags: statistics
---

Put univariate and multivariate Gaussians together for comparison:

**Univariate Gaussian**:

$$
\begin{align}
p(x) 
&= \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left \{- \frac{1}{2} \frac{(x - \mu)^2}{\sigma^2} \right \} \\
&= \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left \{- \frac{1}{2} (x - \mu)\frac{1}{\sigma^2}(x - \mu) \right \} \\
\end{align}
$$


**Multivariate Gaussian**:

$$
\begin{equation}
p(\mathbf{x}) = \frac{1}{\sqrt{(2 \pi)^D |\Sigma|}} \exp \left \{\frac{1}{2} (\mathbf{x} - \boldsymbol{\mu})^T \Sigma^{-1} (\mathbf{x} - \boldsymbol{\mu}) \right \}
\end{equation}
$$


**Conditional distributions**:

Given

$$
\begin{align}
\mathbf{x} 
&= \begin{bmatrix}
\mathbf{x}_a \\ 
\mathbf{x}_b
\end{bmatrix} \\
\boldsymbol{\mu}
&= \begin{bmatrix}
\boldsymbol{\mu}_a \\ 
\boldsymbol{\mu}_b
\end{bmatrix} \\
\Sigma 
&= \begin{bmatrix}
\Sigma_{aa} & \Sigma_{ab} \\ 
\Sigma_{ba} & \Sigma_{bb}
\end{bmatrix} \\
\Lambda &= \begin{bmatrix}
\Lambda_{aa} & \Lambda_{ab} \\ 
\Lambda_{ba} & \Lambda_{bb}
\end{bmatrix} = \Sigma^{-1} \\
\end{align}
$$

Note, $\Sigma$ is the covariance matrix and $\Lambda$ is its inverse, called the
precision matrix. Also note that the partitioned matrices of $\Sigma$ and
$\Lambda$ are note inverse of each other, i.e.
$$\Sigma_{ij} \ne \Lambda_{ij}^{-1}$$.

After some algebra, the conditional mean and variance expressed in terms of partitioned precision matrices:

$$
\begin{align}
\boldsymbol{\mu}_{a|b} &= \boldsymbol{\mu}_a - \Lambda_{aa}^{-1} \Lambda_{ab} (\mathbf{x}_b - \boldsymbol{\mu}_b) \\
\Sigma_{a|b} &= \Lambda_{aa}^{-1} \\
\end{align}
$$

Alternatively, these can be also expressed in terms of partitioned covariance matrices, which is a bit more complex for 
$$\Sigma_{a|b}$$:

$$
\begin{align}
\boldsymbol{\mu}_{a|b} &= \boldsymbol{\mu}_a + \Sigma_{ab} \Sigma_{bb}^{-1} (\mathbf{x}_b - \boldsymbol{\mu}_b) \\
\Sigma_{a|b} &= \Sigma_{aa} - \Sigma_{ab} \Sigma_{aa}^{-1} \Sigma_{ba}
\end{align}
$$

Note
$$\boldsymbol{\mu}_{a|b}$$ is a linear function of $$\mathbf{x}_b$$.

so
$$p(\mathbf{x}_a|\mathbf{x}_b) \sim \mathcal{N}(\boldsymbol{\mu}_{a|b},\Sigma_{a|b})$$.

**Marginal distribution**:

$$
\begin{align}
\mathbb{E}[\mathbf{x}_a] &= \boldsymbol{\mu}_a \\
\text{cov}[\mathbf{x}_a] &= \Sigma_{aa} \\
\end{align}
$$

i.e.
$$p(\mathbf{x}_a) \sim \mathcal{N}(\mathbf{x}_a | \boldsymbol{\mu}_a, \Sigma_{aa})$$, which is straightforward.
