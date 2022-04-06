---
layout: post
title: Compare prediction approaches in classification and regression problems 
author: Zhuyi Xue
tags: machine learning, regression, classification
---

* toc
{:toc}

This post summarizes Page 43 (classification) and Page 47 of
[PRML](https://www.microsoft.com/en-us/research/uploads/prod/2006/01/Bishop-Pattern-Recognition-and-Machine-Learning-2006.pdf).

## Classification

#### Generative model

1. predict joint probability density $p(\mathbf{x}, \mathcal{C}_k)$,
1. then calculate the posterior $p(\mathcal{C}_k \vert \mathbf{x})$,
1. then make a decision.

#### Discriminative model

1. predict the posterior $p(\mathcal{C}_k \vert \mathbf{x})$,
1. then make a decision

#### Discriminant function (model)

1. predict the decision (class label) directly without probability playing a role.

## Regression model

Assuming a mean squared loss is used

$$
E[L]= \iint \{y(\mathbf{x}) − t\}^2 p(\mathbf{x}, t) d\mathbf{x} dt
$$

then, when the loss is minimized

$$
y(\mathbf{x})^* = \arg \min_{y(\mathbf{x})} \mathbb{E}[L] = E[t|\mathbf{x}]
$$

i.e. the expectation conditional expectation of $t$ given the feature vector $\mathbf{x}$.

### Approach 1 (similar to the generative model in classification)

1. predict join probability density $p(\mathbf{x}, t)$,
1. then calculate the posterior $p(t \vert \mathbf{x})$,
1. then calculate $E[t \vert \mathbf{x}]$

### Approach 2 (similar to the discriminative model in classification)

1. predict the posterior $p(t \vert \mathbf{x})$,
1. then calculate  $E[t \vert \mathbf{x}]$

### Approach 3 (similar to the discriminant function in classification)

1. predict  $E[t \vert \mathbf{x}]$ directly without probability playing a role
