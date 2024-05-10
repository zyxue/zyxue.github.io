---
layout: post
title: Gaussian distributions
author: Zhuyi Xue
tags: statistics
---

* toc
{:toc}

Put univariate and multivariate Gaussians together for comparison:

### Univariate Gaussian

$$
\begin{align}
p(x)
&= \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left \{- \frac{1}{2} \frac{(x - \mu)^2}{\sigma^2} \right \} \\
&= \frac{1}{\sqrt{2 \pi \sigma^2}} \exp \left \{- \frac{1}{2} (x - \mu)\frac{1}{\sigma^2}(x - \mu) \right \} \\
\end{align}
$$


### Multivariate Gaussian

$$
\begin{equation}
p(\mathbf{x}) = \frac{1}{\sqrt{(2 \pi)^D |\Sigma|}} \exp \left \{ - \frac{1}{2} (\mathbf{x} - \boldsymbol{\mu})^T \Sigma^{-1} (\mathbf{x} - \boldsymbol{\mu}) \right \}
\end{equation}
$$

If in terms of precision ($\Lambda$):

$$
\begin{equation}
p(\mathbf{x})
= (2\pi)^{-\frac{D}{2}} |\Lambda|^{\frac{1}{2}} \exp \left \{ - \frac{1}{2} (\mathbf{x} - \boldsymbol{\mu})^T \Lambda(\mathbf{x} - \boldsymbol{\mu}) \right \}
\end{equation}
$$


### Conditional distributions

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

### Marginal distribution

$$
\begin{align}
\mathbb{E}[\mathbf{x}_a] &= \boldsymbol{\mu}_a \\
\text{cov}[\mathbf{x}_a] &= \Sigma_{aa} \\
\end{align}
$$

i.e.
$$p(\mathbf{x}_a) \sim \mathcal{N}(\mathbf{x}_a | \boldsymbol{\mu}_a, \Sigma_{aa})$$, which is straightforward.


### Conjugate prior for Gaussian distributions with different unknown parameters

| mean    | variance/precision | dimension    | conjugate prior                                        |
|---------|--------------------|--------------|--------------------------------------------------------|
| unknown | known              | univariate   | univarite Gaussian distribution                        |
| unknown | known              | multivariate | multivariate Gaussian distribution                     |
| known   | unknown            | univariate   | Gamma distribution                                     |
| known   | unknown            | multivariate | Wishart distribution (Multivariate gamma distribution) |
| unknown | unknown            | univariate   | Gaussian-gamma distribution                            |
| unknown | unknown            | multivariate | Gaussian-Wishart distribution                          |

<br>

### Maximum-likelihood estimates

See this [notebook](https://github.com/zyxue/book-notes-pattern-recognition-and-machine-learning-bishop/blob/master/ch2-probability-distributions/ex-2.34-find-maximum-likelihood-estimate-of-covariance-matrix-of-a-multivariate-gaussian.ipynb).


### Moment-Generating Function (MGF)

We derive the MGF of $\mathcal{N}(\mu, \sigma^2)$ as shown below.

$$
\begin{align}
M_X(\lambda)
&= \mathbb{E}[e^{\lambda X}] \\
&= \int \exp(\lambda x) \frac{1}{\sqrt{2\pi \sigma^2}} \exp \left[ -\frac{(x - \mu)^2}{2\sigma^2}\right ] dx \\
&= \frac{1}{\sqrt{2\pi \sigma^2}} \int \exp\left[ -\frac{1}{2\sigma^2} \left(x^2 - 2\mu x + \mu^2 -  2\sigma^2 \lambda x \right ) \right ] dx \\
&= \frac{1}{\sqrt{2\pi \sigma^2}} \int \exp\left[ -\frac{1}{2\sigma^2} \left( \left(x - \mu - \sigma^2 \lambda \right )^2 - \left( \mu + \sigma^2 \lambda \right )^2 + \mu^2  \right ) \right ] dx \\
&=  \exp\left(\frac{\sigma^4 \lambda^2  + 2\mu \sigma^2 \lambda}{2\sigma^2} \right)  \frac{1}{\sqrt{2\pi \sigma^2}} \int \exp\left[ -\frac{1}{2\sigma^2} \left(x - \mu - \sigma^2 \lambda \right )^2  \right ] dx \label{eq:factored} \\
&= \exp\left( \frac{\sigma^2 \lambda^2}{2} + \mu\lambda \right)
\end{align}
$$


Note, in Eq. \eqref{eq:factored}, $\frac{1}{\sqrt{2\pi \sigma^2}} \int \exp\left[ -\frac{1}{2\sigma^2} \left(x - \sigma^2 \lambda \right )^2  \right ]$
is the integration of the distribution of $\mathcal{N}(\mu + \sigma^2 \lambda, \sigma^2)$, so it equals 1.

Therefore,

* for $\mathcal{N}(0, 1)$, $M_X(\lambda) = \exp \frac{\lambda^2}{2}$.
* for $\mathcal{N}(0, \sigma^2)$, $M_X(\lambda) = \exp \frac{\sigma^2 \lambda^2}{2}$.


### Sample statistics

Suppose $X \sim \mathcal{N}(\mu, \sigma^2)$, then given i.i.d. $X_1, \cdots,
X_n$, the sample statistics have the following distribution:

* Sample mean $\bar{X}_n \sim \mathcal{N}(\mu, \frac{\sigma^2}{n})$, and
* Standardize sample mean: $\frac{\bar{X} - \mu}{\sigma / \sqrt{n}} \sim \mathcal{N}(0, 1)$
* Sample variance: $\frac{(n - 1) S_n^2}{\sigma^2} \sim \chi_{n-1}^2$, where  $S_n^2 = \frac{1}{n - 1} \sum_{i=1}^n (X_i - \bar{X})^2$.