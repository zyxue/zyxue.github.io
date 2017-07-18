---
layout: post
title: Notes on common statistics
author: Zhuyi Xue
tags: statistics
---

Being often confused by those concepts, here I take the note. I fixed the usage
of symbols, following the convention, to avoid ambiguity.

### Population-wise statistics

Suppose there are $$N$$ samples in total in the population.

#### Population mean:

\begin{equation}
    \mu = \frac{1}{N}\sum_{i=1}^{N}x_i
    \label{eq:populationMean}
\end{equation}

#### Population variance:

\begin{equation}
    \sigma ^2 = \frac{1}{N}\sum_{i=1}^{N}(x_i - \mu) ^2
    \label{eq:populationVariance}
\end{equation}

#### Population standard deviation:

\begin{equation}
    \sigma = \sqrt{\frac{\sum_{i=1}^{N}(x_i - \overline{x}) ^2}{N}}
    \label{eq:populationStd}
\end{equation}

### Sample-wise statistics

Now suppose we take a sample of sample size $$n$$ from the population.

#### Sample mean

**NOTE**: $$\overline{x}$$ is used instead of $$\mu$$.

\begin{equation}
    \overline{x} = \frac{1}{n}\sum_{i=1}^{n}x_i
    \label{eq:sampleMean}
\end{equation}

#### Sample variance 

$$n-1$$ is used instead of $$n$$ after
[Bessel's correction](https://en.wikipedia.org/wiki/Bessel%27s_correction). The
intuition behind such correction is that sample variance tends to underestimate
population variance, so we intentionally enlarge it a bit. Please see
[Bessel's correction](https://en.wikipedia.org/wiki/Bessel%27s_correction) for more details.

**NOTE**: $$s^2$$ is used instead of $$\sigma ^2$$.

\begin{equation}
    s ^2 = \frac{1}{n - 1}\sum_{i=1}^{n}(x_i - \overline{x}) ^2
    \label{eq:sampleVariance}
\end{equation}

#### Sample standard deviation:

\begin{equation}
    s = \sqrt{\frac{\sum_{i=1}^{n}(x_i - \overline{x}) ^2}{n - 1}}
    \label{eq:sampleStd}
\end{equation}

#### Standard error:

The full name is the standard error of the mean (SEM), which is the expected
value of the standard deviation of means of several samples.

**NOTE**: it is the standard deviation of the sample mean ($$\overline{x}$$),
_NOT_ that of the variable ($$x_i$$).

\begin{equation}
    \textrm{SEM} = \frac{s}{\sqrt n} = \sqrt{\frac{\sum_{i=1}^{n}(x_i - \overline{x}) ^2}{(n - 1)(n)}}
    \label{eq:standardErrorCorrected}
\end{equation}

If without [Bessel's correction](https://en.wikipedia.org/wiki/Bessel%27s_correction):


\begin{equation}
    \textrm{SEM} = \frac{s}{\sqrt n} = \frac{\sqrt{\sum_{i=1}^{n}(x_i - \overline{x}) ^2}}{n}
    \label{eq:standardErrorWithoutCorrection}
\end{equation}
