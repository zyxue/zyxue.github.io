---
layout: post
title: Notes on common statistics
author: Zhuyi Xue
tags: statistics
---

# Laws

**Law of total expectation** (Adam's Law)

\begin{equation}
\mathbb{E}[X] = \mathbb{E}\big[\mathbb{E}[X|N]\big] = \mathbb{E}[\mu N] = \mu\mathbb{E}[N]
\end{equation}

e.g.

* $X$ is the total amount of money all customers spend in a shop in a day
* $N$ is the number of customers visited that shop
* $\mu$ is the mean amount of money a customer spends

**Law of total variance** (Eve's law)

<script type="math/tex; mode=display">% <![CDATA[
\begin{align}
\mathbb{V}[X]
&= \mathbb{E}\big[\mathbb{V}[X|N]\big] + \mathbb{V}\big[\mathbb{E}[X|N]\big] \\
&= \mathbb{E}[N\sigma^2] + \mathbb{V}[\mu N] \\
&= \sigma^2\mathbb{E}[N] + \mu^2\mathbb{V}[N]
\end{align}
%]**></script>

# Inequalities

**Cauchy-Schwarz inequality**

\begin{equation}
\mathbb{E}[XY] \le \sqrt{\mathbb{E}[X^2]\mathbb{E}[Y^2]}
\end{equation}

**Jensen's inequality**

If $f$ is a convex function

\begin{equation}
f\big(\mathbb{E}[X]\big) \le \mathbb{E}[f(x)]
\end{equation}

**Markov's inequality**

\begin{equation}
\text{P}\big(\left|X\right| \ge a\big) \le \frac{\mathbb{E}[X]}{a}
\end{equation}

**Chebyshev's inequality**

\begin{equation}
\text{P}\big(\left|X - \mu\right| \gt a \big) \le \frac{\mathbb{V}[X]}{a^2}
\end{equation}

where $\mu=\mathbb{E}[X]$, and $a \gt 0$.

**Hoeffding's inequality**

TODO, see All of Statistics book

# Approximation

Approximate binomial distribution with 

* Possion distribution when $n$ is large and $p$ is small ($\to 0$).
* Normal distribution when $n$ is large and $p$ is close to $1/2$.

# Population statistics

Suppose there are $$N$$ samples in total in the population.

**Population mean**

\begin{equation}
    \mu = \frac{1}{N}\sum_{i=1}^{N}X_i
    \label{eq:populationMean}
\end{equation}

**Population variance**

\begin{equation}
    \sigma ^2 = \frac{1}{N}\sum_{i=1}^{N}(X_i - \mu) ^2
    \label{eq:populationVariance}
\end{equation}

$\sigma$ is the population standard deviatin.

# Sample statistics

Suppose we take a sample of sample size $$n$$ from the population.

**Sample mean**

*Note*: $$\overline{X}$$ is used instead of $$\mu$$.

\begin{equation}
    \overline{X} = \frac{1}{n}\sum_{i=1}^{n}X_i
    \label{eq:sampleMean}
\end{equation}

**Sample variance**

$$n-1$$ is used instead of $$n$$ after
[Bessel's correction](https://en.wikipedia.org/wiki/Bessel%27s_correction). The
intuition behind such correction is that sample variance tends to underestimate
population variance, so we intentionally enlarge it a bit. Please see
[Bessel's correction](https://en.wikipedia.org/wiki/Bessel%27s_correction) for more details.

*Note*: $$s^2$$ is used instead of $$\sigma ^2$$.

\begin{equation}
    s ^2 = \frac{1}{n - 1}\sum_{i=1}^{n}(X_i - \overline{X}) ^2
    \label{eq:sampleVariance}
\end{equation}

and $s$ is the sample standard deviation.


**Standard error**

The full name is the standard error of the mean (SEM), which is the standard
deviation of sample mean ($$\overline{X}$$), which is still a random variable
(r.v.).

\begin{equation}
    \textrm{SEM} = \frac{s}{\sqrt n}
    \label{eq:standardErrorCorrected}
\end{equation}

# Standardization

[Standardization](https://en.wikipedia.org/wiki/Feature_scaling#Standardization)
is a common transformation that brings data to be centered at 0 with unit standard deviation.

Let's denote the transformed value of $$X_i$$ as $$X_i'$$,

\begin{equation}
    X'_i = \frac{X_i - \overline{X}}{s}
    \label{eq:standardization}
\end{equation}

Apparently, the mean after standardization $$\overline{x'}$$ becomes 0. Let's
calculate the variance of the transformed data,

<script type="math/tex; mode=display">% <![CDATA[
\begin{align}
    s'^2 & = \frac{\sum_{i=1}^{n}(X'_i) ^2}{n - 1} \nonumber \\
         & = \frac{\sum_{i=1}^{n}(\frac{X_i - \overline{X}}{s}) ^2}{n - 1}                                                     & \text{replacing}\ X_i'\ \text{with Eq. \eqref{eq:standardization}} \nonumber \\
         & = \frac{1}{s^2} \frac{\sum_{i=1}^{n}({X_i - \overline{X}}) ^2}{n - 1}                                               & \text{move constant}\  s^2\ \text{to the front} \nonumber \\
         & = \frac{n - 1}{\sum_{i=1}^{n}({X_i - \overline{X}}) ^2} \frac{\sum_{i=1}^{n}({X_i - \overline{X}}) ^2}{n - 1}       & \text{replacing }\ s^2\ \text{with Eq. \eqref{eq:sampleVariance}} \nonumber \\
         & = 1 \nonumber \\
         \label{eq:standarizedSampleStd}
\end{align}
%]]></script>


<!-- <script type="math/tex; mode=display">% <![CDATA[ -->
<!-- \begin{align} -->
<!-- \nabla_{\theta} E_x[f(x)] &= \nabla_{\theta} \sum_x p(x) f(x) & \text{definition of expectation} \\ -->
<!-- & = \sum_x \nabla_{\theta} p(x) f(x) & \text{swap sum and gradient} \\ -->
<!-- & = \sum_x p(x) \frac{\nabla_{\theta} p(x)}{p(x)} f(x) & \text{both multiply and divide by } p(x) \\ -->
<!-- & = \sum_x p(x) \nabla_{\theta} \log p(x) f(x) & \text{use the fact that } \nabla_{\theta} \log(z) = \frac{1}{z} \nabla_{\theta} z \\ -->
<!-- & = E_x[f(x) \nabla_{\theta} \log p(x) ] & \text{definition of expectation} -->
<!-- \end{align} % -->
<!-- ]]></script> -->
