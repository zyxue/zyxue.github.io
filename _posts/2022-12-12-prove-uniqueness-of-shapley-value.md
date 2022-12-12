---
layout: post
title: Prove uniqueness of Shapley value
author: Zhuyi Xue
tags: statistics
---

* toc
{:toc}

## Introduction

Consider the problem that a set of players $N$ form a coalition to play a game
$v$, and obtain some payoff, how should the payoff been divided and allocated
to each player $i$. Here, $v$ is a set function: $2^{n-1} \rightarrow
\mathbb{R}$, i.e. it maps any subset of $N$ to a numerical value, which can be
interpreted as the payoff if just the subset of players play the game.

[Shapley value](https://en.wikipedia.org/wiki/Shapley_value) is a solution to
such problem, and it is usually written as

$$
\begin{align}
\phi_i(v)
&= \sum _{S \subseteq N \backslash \{i\}} \frac{s!(n - s - 1)!}{n!} \Big(v(S \cup \{i\}) - v(S) \Big) \tag{1} \\
\end{align}
$$

where

* $S \subset N$
* $s = \|S\|$
* $n = \|N\|$
* $i$ means the $i$th player
* $\phi_i(N, v)$ means the payoff allocated to player $i$ in the context of coalition $N$ and game $v$.

Shapley value is a unique solution that satisfies three fairness-related axioms[^1]. This
post shows the proof of such uniqueness in detail.

We first introduce the three axioms, and then show how they lead to the unique
solution.

At this point, we'll assume we don't know the exact formula and just refer it
as a value function, $\phi_i(v)$.

## Fairness axioms

### Axiom 1: Symmetry

Let $\pi$ be any permutation of $N$, $\phi$ should satisfy

$$
\begin{align}
\phi_{\pi(i)} (\pi v) = \phi_i(v)
\end{align}
$$

where $i$ and $\pi(i)$ are the indice before and after permutation, respectively.


### Axiom 2: Carrier

which implies Dummy and Efficiency axioms in alternative formulation of the axioms.

### Axiom 3: Additivity


[^1]: Depending on how the axioms are formulated, they can also be four instead of three axioms. In this post, we follow the formulation as originally defined in Shapley 1953.

# Reference

* Shapley, L. (1953). A value for n-person games. Contributions to the Theory of Games, 2(28), 307–317. ([pdf](http://neconomides.stern.nyu.edu/networks/Shapley_A_value_for_n-person_games.pdf))
