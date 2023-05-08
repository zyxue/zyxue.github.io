---
layout: post
title: Notes on measure theory and probability
author: Zhuyi Xue
tags: probability
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
});
</script>

Definitions:

* $\Omega$: **sample space**.
* $\omega$: **sample outcomes** (aka. outcomes, sample points, realization, elements).
* $A$: **event**, subset of $\Omega$, $A \subseteq \Omega$.
* $\mathcal{A}$: **$\sigma$-algebra**, aka. $\sigma$-field, a set of events that
  satisfy three criteria:
  * $\emptyset \in \mathcal{A}$.
  * if $A \in \mathcal{A}$, then $A^c \in \mathcal{A}$.
  * If $A_i \in \mathcal{A}$ for $i \in \mathbb{N}$, then $\cup_i A_i \in
    \mathcal{A}$ (countable unions).
* $\mathbb{P}: \mathcal{A} \rightarrow \mathbb{R}$, probability distribution,
  aka. **probability measure**, that maps an event $A$ to $[0, 1]$. $\forall A
  \subseteq \Omega, 1 \ge \mathbb{P}(A) \ge 0$. $\mathbb{P}(\emptyset) = 0,
  \mathbb{P}(\Omega) = 1$. Note, the elements in $\mathcal{A}$ are called
  measurable sets. They're measurable in the sense that $\mathbb{P}$ can assign
  value for them. Without restriction to probability, the quantity is just
  called **measure**.
* $(\Omega, \mathcal{A})$: measurable space.
* $(\Omega, \mathcal{A}, \mathbb{P})$: **probability space**. Aka. measure space if
  without restriction to probability.
* $X: \Omega \rightarrow \mathbb{R}$: **random variable**, a mapping from a sample
  outcome to a real number.
* $$X^{-1}(x) = \{\omega \in \Omega: X(\omega) = x\}$$, known as the **preimage**.
  Basically, $X^{-1}$ maps an value back to the event, denoted as $A$, in which
  all elements satisfy $X(\omega) = x$.
* The relatinoship between $\mathbb{P}$ and $X$: we denote $$\mathbb{P}(\{
  \omega \in \Omega: X(\omega) = x\}) = \mathbb{P}(A) =\mathbb{P}(X=x)$$. Here,
  $X=x$ is just a shorthand, meaning the set of all $\omega$ with $X(\omega) =
  x$, denoted by $A$.
* A orobability model consists of three components (Ref:
  [Chapter 1 notes](https://ocw.mit.edu/courses/6-262-discrete-stochastic-processes-spring-2011/6d0c6fbaf3afa16cbc76fc1f27d1d34e_MIT6_262S11_chap01.pdf) of [Discrete Stochastic Processes course](https://ocw.mit.edu/courses/6-262-discrete-stochastic-processes-spring-2011/pages/course-notes/)):
  1. A sample space $\Omega$.
  1. A class of events $\mathcal{A}$.
  1. A probability measure $\mathbb{P}$.

Take the stochastic process of throwing a coin twice for example,

* $$\Omega = \{HH, HT, TH, TT\}$$.
* E.g. $\omega = HH$, if both throw ends in head.
* E.g. $$A = \{HH, HT\}$$, i.e. all events with the first throw in head.
* $$\mathbb{P}(\{HH, HT\}) = 2 / 4$$, i.e. the probability of having the
  first throw in head. In comparison, the probability of having at least one
  head is $$\mathbb{P}(\{HH, HT, TH\}) = 3 / 4$$.
* Let $X$ be the number of heads, then $X(HH) = 2$, $X(HT) = 1$, $X(TT) = 0$,
  e.g. $$\mathbb{P}(X = 1) = \mathbb{P}(\{HT, TH\}) = 2 / 4$$. $\mathbb{P}(X =
  1)$ known as the probability of having one head, its value can be calculated
  by dividing the number of all elements with one head divided by that of all
  elements. All the elements with one head consistute the event $A$ we're interested
  in here.

