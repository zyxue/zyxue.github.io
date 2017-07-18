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


### Standardization

[Standardization](https://en.wikipedia.org/wiki/Feature_scaling#Standardization)
is a very common data transformation in machine learning. Let's use $$x'$$ as the
transformed value, and prove how it works

The transformation:

\begin{equation}
    x'_i = \frac{x_i - \overline{x}}{s}
    \label{eq:standardization}
\end{equation}

Apparently, the mean after standardization $$\overline{x'}$$ becomes 0. What
about the variance $$s' ^2$$.

<!-- reference: view-source:http://karpathy.github.io/2016/05/31/rl/ -->
<!-- '\ ' force a space -->

<script type="math/tex; mode=display">% <![CDATA[
\begin{align}
    s'^2 & = \frac{\sum_{i=1}^{n}(x'_i - 0) ^2}{n - 1} \nonumber \\
         & = \frac{\sum_{i=1}^{n}(\frac{x_i - \overline{x}}{s}) ^2}{n - 1}                                                     & \text{replacing}\ x_i'\ \text{with Eq. \eqref{eq:standardization}} \nonumber \\
         & = \frac{\frac{1}{s^2} \sum_{i=1}^{n}({x_i - \overline{x}}) ^2}{n - 1}                                               & \text{move constant}\  s^2\ \text{to the front} \nonumber \\
         & = \frac{\frac{n - 1}{\sum_{i=1}^{n}({x_i - \overline{x}}) ^2} \sum_{i=1}^{n}({x_i - \overline{x}}) ^2}{n - 1}       & \text{replacing }\ s^2\ \text{with Eq. \eqref{eq:sampleStd}} \nonumber \\
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
