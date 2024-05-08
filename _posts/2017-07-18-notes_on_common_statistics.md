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

Suppose $X$ is a non-negative random variable, $f$ is the pdf and $a > 0$, then

$$
\begin{equation}
\mathbb{P}\big(X > a\big) \le \frac{\mathbb{E}[X]}{a}
\end{equation}
$$

*Proof*:

$$
\begin{align}
\mathbb{E}[X]
&= \int_0^\infty x f(x) dx \\
&= \int_0^a x f(x) dx + \int_a^\infty x f(x) dx \label{eq:markov_before_inequality} \\
&\ge 0 + \int_a^\infty a f(x) dx \label{eq:markov_to_inequality} \\
&\ge a \int_a^\infty f(x) dx \\
&= a \mathbb{P}(X > a) \\
\mathbb{P}(X > a)
&\le \frac{\mathbb{E}[X]}{a}
\end{align}
$$

Note, from Eq. $\eqref{eq:markov_before_inequality}$ to
$\eqref{eq:markov_to_inequality}$, we just used the simple facts:

* $\int_0^a x f(x) dx \ge 0$ and
* $x \ge a$ in the integral $\int_a^\infty x f(x) dx$.

The reson that $X$ needs to be non-negative is that otherwise the first
integral would become $\int_{-\infty}^{a} x f(x) dx$, which isn't necessarily $\ge
0$.

**Chebyshev's inequality (specific version)**

\begin{equation}
\mathbb{P}\left( \left|X - \mu \right| \gt a \right) \le \frac{\mathbb{V}[X]}{a^2}
\end{equation}

where $\mu=\mathbb{E}[X]$, and $a \gt 0$.

*Proof*:

We can derive Chebyshev's in equality from applying the Markov's inequality:

$$
\begin{align}
\frac{\mathbb{V}[X]}{a^2}
&=\frac{\mathbb{E}\left[ (X - \mu)^2 \right ]}{a^2} \label{eq:chebyshev_def_var} \\
&\ge \mathbb{P}\left( (X - \mu)^2 > a^2 \right ) \label{eq:chebyshev_apply_markov} \\
&= \mathbb{P}(|X - \mu| > a )
\end{align}
$$

Note,

* Eq. $\eqref{eq:chebyshev_def_var}$ is just the definition of variance.
* Eq. $\eqref{eq:chebyshev_apply_markov}$ is the application of Markov's inequality
  with $(X - \mu)^2$ as the random variable.

**Chebyshev's inequality (general version)**

$$
\begin{align*}
\mathbb{P}(g(X) \ge r) \le \frac{\mathbb{E}[g(X)]}{r}
\end{align*}
$$

where $g(X)$ is a non-negative function, and $r > 0$.

*Proof*:

$$
\begin{align}
\mathbb{E}[g(X)]
&= \int_{-\infty}^{\infty} g(x) f(x) dx \\
&= \int_{g(X) < r} g(x) f(x) dx + \int_{g(X) \ge r} g(x) f(x) dx \\
&\ge 0 + r \int_{g(X) > r} f(x) dx \\
&= r\mathbb{P} \left( g(X) > r \right) \\
\mathbb{P} \left( g(X) > r \right)
&\le \frac{\mathbb{E}[g(X)]}{r}
\end{align}
$$

The proof uses the same idea as that for Markov's inequality, but more general.

* When $g(X) = |X|$, the the general-version Chebyshev's inequality becomes
  Markov's inequality.
* When $g(X) = (X - \mu)^2$, then it becomes the specific-version Chebyshev's
  inequality

**Proof**

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
