---
layout: post
title: Notes on common statistics
author: Zhuyi Xue
tags: statistics
---

* toc
{:toc}

## Laws

### Law of total expectation (Adam's Law)

\begin{equation}
\mathbb{E}[X] = \mathbb{E}\big[\mathbb{E}[X|N]\big] = \mathbb{E}[\mu N] = \mu\mathbb{E}[N]
\end{equation}

e.g.

* $X$ is the total amount of money all customers spend in a shop in a day
* $N$ is the number of customers visited that shop
* $\mu$ is the mean amount of money a customer spends

### Law of total variance (Eve's law)

<script type="math/tex; mode=display">% <![CDATA[
\begin{align}
\mathbb{V}[X]
&= \mathbb{E}\big[\mathbb{V}[X|N]\big] + \mathbb{V}\big[\mathbb{E}[X|N]\big] \\
&= \mathbb{E}[N\sigma^2] + \mathbb{V}[\mu N] \\
&= \sigma^2\mathbb{E}[N] + \mu^2\mathbb{V}[N]
\end{align}
%]**></script>

## Inequalities

### Cauchy-Schwarz inequality

\begin{equation}
\mathbb{E}[XY] \le \sqrt{\mathbb{E}[X^2]\mathbb{E}[Y^2]}
\end{equation}

### Jensen's inequality

If $f$ is a convex function

\begin{equation}
f\big(\mathbb{E}[X]\big) \le \mathbb{E}[f(x)]
\end{equation}

### Markov's inequality

Suppose $X$ is a non-negative random variable, $f$ is the pdf and $a > 0$, then

$$
\begin{equation}
\mathbb{P}\big(X \ge a\big) \le \frac{\mathbb{E}[X]}{a}
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
&= a \mathbb{P}(X \ge a) \\
\mathbb{P}(X \ge a)
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

### Chebyshev's inequality (specific version)

\begin{equation}
\mathbb{P}\left( \left|X - \mu \right| \ge a \right) \le \frac{\mathbb{V}[X]}{a^2}
\end{equation}

where $\mu=\mathbb{E}[X]$, and $a \gt 0$.

*Proof*:

We can derive Chebyshev's in equality from applying the Markov's inequality:

$$
\begin{align}
\frac{\mathbb{V}[X]}{a^2}
&=\frac{\mathbb{E}\left[ (X - \mu)^2 \right ]}{a^2} \label{eq:chebyshev_def_var} \\
&\ge \mathbb{P}\left( (X - \mu)^2 \ge a^2 \right ) \label{eq:chebyshev_apply_markov} \\
&= \mathbb{P}(|X - \mu| \ge a )
\end{align}
$$

Note,

* Eq. $\eqref{eq:chebyshev_def_var}$ is just the definition of variance.
* Eq. $\eqref{eq:chebyshev_apply_markov}$ is the application of Markov's inequality
  with $(X - \mu)^2$ as the random variable.

### Chebyshev's inequality (general version)

$$
\begin{align}
\mathbb{P}(g(X) \ge r) \le \frac{\mathbb{E}[g(X)]}{r}
\end{align}
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
**Proof*

### Hoeffding's inequality

TODO, see All of Statistics book

## Properties of random sample

Suppose we have an i.i.d. sample, $X_1, \cdots, X_n$, from a population with
mean $\mu$ and variance $\sigma^2$.

### Sample mean

$$
\begin{align}
\bar{X}_n
&= \frac{1}{n}\sum_{i=1}^{n}X_i \label{eq:sampleMean}
\end{align}
$$

The subscript $_n$ just means it is the mean of a sample of size $n$. We can show
that $\bar{X}_n$ converges to $\mu$ when $n \rightarrow \infty$, given $\sigma^2
< \infty$.

*Proof*:

$$
\begin{align}
\lim_{n \rightarrow \infty} \mathbb{P}(|\bar{X}_n - \mu| \ge \epsilon)
&\le \lim_{n \rightarrow \infty} \frac{\mathbb{V}[\bar{X}_n - \mu]}{\epsilon^2} \label{eq:lln_chebyshev} \\
&= \lim_{n \rightarrow \infty} \frac{\sigma^2/n}{\epsilon} \\
&= \lim_{n \rightarrow \infty} \frac{\sigma^2}{n\epsilon} \\
&= 0 \\
\end{align}
$$

Equivalently,

$$
\begin{align}
\lim_{n \rightarrow \infty} \mathbb{P}(|\bar{X}_n - \mu| < \epsilon) \label{eq:lln_equivalent}
&= 1
\end{align}
$$

Note, Eq. $\eqref{eq:lln_chebyshev}$ is an application of the Chebyshev's
inequality. This property of $\bar{X}_n$ is known as the **weak law of large
numbers** (*large number* refers to a *large sample size*).

**Important properties**, $\mathbb{E}[\bar{X}_n] = \mu$, $\mathbb{V}[\bar{X}_n]
= \sigma^2 / n$. By central limit theorem, when $n$ is large, $\bar{X}_n \sim
\mathcal{N} \left(\mu, \frac{\sigma^2}{n} \right)$.

### Sample variance

$$
\begin{align}
S_n ^2
&= \frac{1}{n - 1}\sum_{i=1}^{n}(X_i - \bar{X}) ^2 \label{eq:sampleVariance}
\end{align}
$$

Note, the denominator uses $n - 1$ instead of $n$, which would lead to a biased
estimator (see proof of bias
[here](https://stats.stackexchange.com/a/544455/112141)). This modification is
also called [Bessel's
correction](https://en.wikipedia.org/wiki/Bessel%27s_correction).

### Sample standard deviation

$$
\begin{align}
S_n = \sqrt{S_n^2} = \sqrt{\frac{1}{n - 1}\sum_{i=1}^{n}(X_i - \bar{X}) ^2}
\end{align}
$$

While $S_n^2$ is an unbiased estimator of $\sigma^2$, i.e. $\mathbb{E}[S_n^2] =
\sigma_n^2$, $S_n$ is a biased estimator of $\sigma$, in particular $\mathbb{E}[S_n] \le
\sigma$.

*Proof*:

$$
\begin{align}
\mathbb{V}[S_n]
&= \mathbb{E}[S_n^2] - (\mathbb{E}[S_n])^2 \\
&= \sigma^2 - (\mathbb{E}[S_n])^2 \\
&= (\sigma + \mathbb{E}[S_n])(\sigma - \mathbb{E}[S_n]) \\
\end{align}
$$

Given $\mathbb{V}[S] \ge 0$ and $(\sigma + \mathbb{E}[S_n]) \ge 0$, so $\sigma -
\mathbb{E}[S_n] \ge 0$.

### Standard error of sample mean (SEM)

SEM is just the standard deviation of the sample mean $\bar{X}_n$, i.e.

$$
\begin{align}
\textrm{SEM}_n
&= \sqrt{\mathbb{V}\left[ \bar{X}_n \right]} =\frac{\sigma}{\sqrt{n}}
\end{align}
$$

and it can be estimated with $\widehat{\textrm{SEM}_n} = S_n/\sqrt{n}$. (TODO: analyze the property of this estimator.)

Note, do not confuse $\textrm{SEM}_n^2$ with sample variance $S_n^2$. The former is
the variance of the sample mean, a fixed number, while the later is an estimator
of the population variance, a random variable.

### Standardization

[Standardization](https://en.wikipedia.org/wiki/Feature_scaling#Standardization)
is a common transformation that brings data to be centered at 0 with unit standard deviation.

Let's denote the transformed value of $$X_i$$ as $$X_i'$$,

\begin{equation}
    X'_i = \frac{X_i - \bar{X}}{s}
    \label{eq:standardization}
\end{equation}

Apparently, the mean after standardization $$\bar{x'}$$ becomes 0. Let's
calculate the variance of the transformed data,

<script type="math/tex; mode=display">% <![CDATA[
\begin{align}
    s'^2 & = \frac{\sum_{i=1}^{n}(X'_i) ^2}{n - 1} \nonumber \\
         & = \frac{\sum_{i=1}^{n}(\frac{X_i - \bar{X}}{s}) ^2}{n - 1}                                                     & \text{replacing}\ X_i'\ \text{with Eq. \eqref{eq:standardization}} \nonumber \\
         & = \frac{1}{s^2} \frac{\sum_{i=1}^{n}({X_i - \bar{X}}) ^2}{n - 1}                                               & \text{move constant}\  s^2\ \text{to the front} \nonumber \\
         & = \frac{n - 1}{\sum_{i=1}^{n}({X_i - \bar{X}}) ^2} \frac{\sum_{i=1}^{n}({X_i - \bar{X}}) ^2}{n - 1}       & \text{replacing }\ s^2\ \text{with Eq. \eqref{eq:sampleVariance}} \nonumber \\
         & = 1 \nonumber \\
         \label{eq:standarizedSampleStd}
\end{align}
%]]></script>

## Convergence


Convergence in distribution:

$$
\begin{align}
\lim_{n \rightarrow \infty} F_{X_n}(x) &= F_X(x) \\
\end{align}
$$

which is denoted as $X_n \stackrel{F}{\longrightarrow} X$, where $F$ is the cdf.

Convergence in probability:

$$
\begin{align}
\lim_{n \rightarrow \infty} \mathbb{P}(|X_n - X| < \epsilon) &= 1 \\
\end{align}
$$

which is denoted as $X_n \stackrel{\mathbb{P}}{\longrightarrow} X$.

Convergence almost surely:

$$
\begin{align}
\mathbb{P} \left(\lim_{n \rightarrow \infty} |X_n - X| < \epsilon \right) &= 1 \\
\end{align}
$$

which is denoted as $X_n \stackrel{a.s.}{\longrightarrow} X$.


In general:

* $X_n \stackrel{a.s.}{\longrightarrow} X$ => $X_n
  \stackrel{\mathbb{P}}{\longrightarrow} X$ (Sufficent).
* $X_n \stackrel{\mathbb{P}}{\longrightarrow} X$ => $X_n
  \stackrel{F}{\longrightarrow} X$ (Sufficent).
* $X_n \stackrel{F}{\longrightarrow} X$ <=> $X_n \stackrel{\mathbb{P}}{\longrightarrow} c$
  (Sufficent and necessary, $c$ is a constant).

*Proof for $X_n \stackrel{\mathbb{P}}{\longrightarrow} X$ => $X_n \stackrel{F}{\longrightarrow} X$:

Strategy: we derive both a lower bound and an upper bound for $F_{X_n}(x)$ given
$X_n \stackrel{\mathbb{P}}{\rightarrow} X$. Let $\epsilon > 0$.

Lower bound:

$$
\begin{align}
F_{X}(x - \epsilon)
&= \mathbb{P}(X \le x - \epsilon) \\
&= \mathbb{P}(X \le x - \epsilon, X_n \le x) + \mathbb{P}(X \le x - \epsilon, X_n > x) \\
&\le \mathbb{P}(X_n \le x) + \mathbb{P}(|X - X_n| > \epsilon) \\
&= F_{X_n}(x) + \mathbb{P}(|X - X_n| > \epsilon) \\
\end{align}
$$

Upper bound:

$$
\begin{align}
F_{X_n}(x)
&= \mathbb{P}(X_n \le x) \\
&= \mathbb{P}(X_n \le x, X \le x + \epsilon) + \mathbb{P}(X_n \le x, X > x + \epsilon) \\
&\le \mathbb{P}(X \le x + \epsilon) + \mathbb{P}(|X - X_n| > \epsilon) \\
&= F_X(x + \epsilon) + \mathbb{P}(|X - X_n| > \epsilon) \\
\end{align}
$$

Therefore,

$$
\begin{align}
F_{X_n}(x)
&\ge F_X(x - \epsilon) - \mathbb{P}(|X - X_n| > \epsilon) \\
F_{X_n}(x)
&\le F_X(x + \epsilon) + \mathbb{P}(|X - X_n| > \epsilon) \\
\end{align}
$$

In the limit, because $X_n \stackrel{\mathbb{P}}{\rightarrow} X$, $\lim_{n
\rightarrow \infty} \mathbb{P}(|X - X_n| > \epsilon) = 0$, then take $\epsilon
\rightarrow 0$, we have $\lim_{n \rightarrow \infty} F_{X_n}(x) = F_X(x)$.

In the case when $X$ is a constant, i.e. $X = c$, then with $\epsilon
\rightarrow 0$,

* if $x < c$, then $F_X(x - \epsilon) = F_X(x + \epsilon) = 0$, so
$F_{X_n}(x) = 0$;
* if $x = c$, then $F_X(x - \epsilon) = 0, F_X(x + \epsilon) =
1$, so $0 \le F_{X_n} \le 1$;
* if $x > c$, then $F_X(x - \epsilon) = F_X(x + \epsilon) = 1$, so $F_{X_n}(x) =
1$.

So the property $X_n \stackrel{\mathbb{P}}{\longrightarrow} c$ => $X_n
  \stackrel{F}{\longrightarrow} c$ still holds.

## Approximation

Approximate binomial distribution with

* Possion distribution when $n$ is large and $p$ is small ($\to 0$).
* Normal distribution when $n$ is large and $p$ is close to $1/2$.
