---
layout: post
title: Prove central limit theory
author: Zhuyi Xue
tags: statistics
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
});
</script>

The **Central Limit Theorem (CLT) says if $X_1, \cdots, X_n$ are iid random
variables with with mean $\mu$ and variance $\sigma^2$, then the distribution of
$$\bar{X}_n = \frac{1}{N} \sum_{i=1}^n X_i$$ converges to a normal distribution, i.e.

$$
\begin{align}
\bar{X}_n \stackrel{F}{\longrightarrow} \mathcal{N} \left(\mu, \frac{\sigma^2}{n} \right) \label{eq:clt_bar_X_n_distribution}
\end{align}
$$

Note, Eq. $\eqref{eq:clt_bar_X_n_distribution}$ is equivalent to

$$
\begin{align}
Z_n = \frac{\bar{X}_n - \mu}{\sigma/\sqrt{n}} \stackrel{F}{\longrightarrow} \mathcal{N} \left(0, 1 \right) \label{eq:clt_Z_n_distribution}
\end{align}
$$

The proof for equivalence is in the Appendix. We'll prove Eq.
$\eqref{eq:clt_Z_n_distribution}$ instead.

Denote $Y_i = \frac{X_i - \mu}{\sigma}$, by properties of expectation,
$\mathbb{E}[Y_i]=0$ and $\mathbb{V}[Y_i]=1$. Then, $Z_n$ can be written as

$$
\begin{align}
Z_n
&= \sqrt{n}{\frac{\bar{X}_n - \mu}{\sigma}} \\
&= \sqrt{n}{\frac{\frac{1}{n} \sum_{i=1}^nX_i - \mu}{\sigma}} \\
&= \sqrt{n}{\frac{1}{n} \sum_{i=1}^n \frac{X_i - \mu}{\sigma}} \\
&= \frac{1}{\sqrt{n}} \sum_{i=1}^n Y_i \\
\end{align}
$$

Suppose $M_Y(t)$, the moment generating function (MGF) of $Y_i$, exists at
around $t=0$. Note, by [properties of
MGF](https://zyxue.github.io/2024/05/11/moment-generating-function.html) that
$M_{cY}(t) = M_{Y}(ct)$ and $M_{Y_i + Y_j}(t) = M_{Y_i}(t)M_{Y_j}(t)$, the MGF
of $Z_n$ can be derived as

$$
\begin{align}
M_{Z_n}(u)
&= M_{\sum_{i=1}^n \frac{1}{\sqrt{n}} Y_i} \\
&= \left( M_{\frac{1}{\sqrt{n}} Y_i} \right)^n \\
&= \left(M_Y \left(\frac{t}{\sqrt{n}} \right )\right)^n  \\
\end{align}
$$

We can expand the base in the exponential with [Taylor expansion](https://zyxue.github.io/2021/09/02/taylor-series.html),

$$
\begin{align}
M_Y \left(\frac{t}{\sqrt{n}} \right)
&= M_Y(0) + \frac{M_Y^{(1)}(0) n^{-\frac{1}{2}}}{1!}t   + \frac{M_Y^{(2)}(0)  n^{-1}}{2!}t^2 + \frac{M_Y^{(3)}(0) n^{-\frac{3}{2}}}{3!}t^3  + \cdots \label{eq:taylor} \\
&= 1 + \frac{n^{-1} t^2}{2} + \frac{M_Y^{(3)}(0) n^{-\frac{3}{2}}}{3!}t^3 + \cdots \label{eq:taylor_simplify}\\
\end{align}
$$

Note,

* In Eq. $\eqref{eq:taylor}$, we also need to take derivative of
  $\frac{t}{\sqrt{n}}$ wrt. $t$ when calculating derivatives of the MGF, which
  is why the parts of $n^{\frac{-k}{2}}$ appear.
* In Eq. $\eqref{eq:taylor_simplify}$, we used the fact that
  * $M_Y(0) = \mathbb{E}[e^0] = 1$
  * $M_Y^{(1)}(0) = \mathbb{E}[Y_i] = 0$
  * $M_Y^{(2)}(0) = \mathbb{V}[Y_i] = 1$

Therefore, in the limit,

$$
\begin{align}
\lim_{n \rightarrow \infty}
M_{Z_n}(u)
&= \lim_{n \rightarrow \infty} \left( M_Y \left(\frac{t}{\sqrt{n}} \right) \right )^n \\
&= \lim_{n \rightarrow \infty} \left( 1 + \frac{n^{-1} t^2}{2} + \frac{M_Y^{(3)}(0) n^{-\frac{3}{2}}}{3!}t^3 + \cdots \right )^n \\
&= \lim_{n \rightarrow \infty} \left( 1 + \frac{t^2/2}{n} \right )^n \label{eq:M_Zn_omit_higher_order_terms}\\
&= e^{t^2 / 2} \label{eq:M_Zn_final_result}
\end{align}
$$

Note,

* in Eq. $\eqref{eq:M_Zn_omit_higher_order_terms}$, we omitted higher order
  terms as they vanishes relative to the second-order term as $n \rightarrow
  \infty$.
* in Eq. $\eqref{eq:M_Zn_final_result}$, we used the fact that $\lim_{n
  \rightarrow \infty} \left(1 + \frac{x}{n} \right )^n = e^n$, which is proved
  in Appendix.

Because $e^{t^2/2}$ is the MGF of a standard normal distribution (see proof
[here](https://zyxue.github.io/2021/08/29/gaussian-distributions.html#moment-generating-function-mgf)),
we've proved the CLT.

**Some thoughts** related to expectation, LLN and CTL:

* Given a sample $X_1, \cdots, X_n$, we know that the expectations
  $\mathbb{E}[\bar{X}_n] = \mu$ and $\mathbb{V}[\bar{X}_n] =
  \frac{\sigma^2}{n}$, but this is more related to the behavior of $\bar{X}_n$
  when the number of samples ($m$) goes to infinity, while CLT is more about how
  $\bar{X}_n$ behaves when the sample size $n$ increases.
* From law of large numbers (i.e. large sample sizes, LLN), we know that
  $\bar{X} \stackrel{\mathbb{P}}{\longrightarrow} \mu$, i.e. a point mass, when
  $n \rightarrow \infty$. CLT shows that in the limit, $\frac{\sigma^2}{n}$ from
  Eq. $\eqref{eq:clt_bar_X_n_distribution}$ goes to $0$, which means the normal
  distribution also becomes a point mass, so they're consistent.

Note

## Appendix

### Prove $\bar{X}_n \stackrel{F}{\longrightarrow} \mathcal{N} \left(\mu, \frac{\sigma^2}{n} \right)$ is equivalent to $\frac{\bar{X}_n - \mu}{\sigma/\sqrt{n}} \stackrel{F}{\longrightarrow} \mathcal{N} \left(0, 1 \right)$

By definition of convergence in distribution, we have

$$
\begin{align}
\lim_{n \rightarrow \infty} \mathbb{}P(\bar{X}_n \le x)
&= \int_{-\infty}^x \frac{1}{\sqrt{2 \pi \sigma^2 / n}} e^{-\frac{1}{2}\frac{(t - \mu)^2}{\sigma^2}} dt \\
&= \int_{-\infty}^{\frac{x - \mu}{\sigma/\sqrt{n}}} \frac{1}{\sqrt{2 \pi}} e^{-\frac{v^2}{2}} dv \label{eq:replace_t_with_v}\\
&= \Phi\left(\frac{x - \mu}{\sigma/\sqrt{n}}\right)
\end{align}
$$

Note,

* In Eq. $\eqref{eq:replace_t_with_v}$ we replaced $t$ with $v =
\frac{t - \mu}{\sigma / \sqrt{n}}$.
* $\Phi$ denotes the cdf of the standard normal distribution.

The LHS can also be rewritten as $\lim_{n \rightarrow \infty} \mathbb{}P
\left(\frac{\bar{X}_n - \mu}{\sigma / \sqrt{n}} \le \frac{x -
\mu}{\sigma/\sqrt{n}} \right)$ by transforming both sides of the inequality, so

$$
\begin{align}
\lim_{n \rightarrow \infty} \mathbb{}P \left(\frac{\bar{X}_n - \mu}{\sigma / \sqrt{n}} \le \frac{x - \mu}{\sigma/\sqrt{n}} \right) = \Phi\left(\frac{x - \mu}{\sigma/\sqrt{n}}\right)
\end{align}
$$

If we denote $z = \frac{x - \mu}{\sigma / \sqrt{n}}$, then

$$
\begin{align}
\lim_{n \rightarrow \infty} \mathbb{}P \left(\frac{\bar{X}_n - \mu}{\sigma / \sqrt{n}} \le z \right) = \Phi(z) \\
\end{align}
$$

which is the definition of $\frac{\bar{X}_n - \mu}{\sigma/\sqrt{n}}
\stackrel{F}{\longrightarrow} \mathcal{N} \left(0, 1 \right)$.

### Prove $\lim_{n \rightarrow \infty} \left(1 + \frac{x}{n} \right )^n = e^n$

By [definition of $e$](https://en.wikipedia.org/wiki/E_(mathematical_constant)),

$$
\begin{align}
e = \lim_{n \rightarrow \infty} \left(1 + \frac{1}{n} \right )^n
\end{align}
$$

Let $\frac{1}{t} = \frac{x}{n}$, so $n=tx$, then we have

$$
\begin{align*}
\lim_{n \rightarrow \infty} \left(1 + \frac{x}{n} \right )^n
&= \lim_{n \rightarrow \infty} \left(1 + \frac{1}{t} \right )^{tx} \\
&= \left( \lim_{n \rightarrow \infty} \left(1 + \frac{1}{t} \right )^t \right )^x \\
&= e^x
\end{align*}
$$
