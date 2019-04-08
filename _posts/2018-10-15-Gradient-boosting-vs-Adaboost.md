---
layout: post
title: Gradient boosting vs Adaboost
author: Zhuyi Xue
tags: machine learning, boosting
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
});
</script>


Gradient boosting (GB) is as generatlization to Adaboost. They both work as
stagewise additive models \eqref{eq:additiveModel}, which means they are an ensemble of weaker learners
(e.g. tree stumps) and one tree is learned after another without changing the
trees previously trained, in a sequential way \eqref{eq:fsam}.

To put this description in equations. The ensemble model is 

\begin{equation}
    f(x) = \sum_{i=1}^{M} \beta_{i}b_i(x;\gamma_i)
    \label{eq:additiveModel}
\end{equation}

where $f(x)$ is the final model, which is an ensemble of $M$ weak learners
$b_i(x, \gamma_i)$, each is weighted by $\beta_i$.

The sequential training process is 

\begin{equation}
    f_m(x) = f_{m-1}(x) + \beta b(x;\gamma)
    \label{eq:fsam}
\end{equation}

where $f_{m-1}$ means $m-1$ weak learners have been trained, and when the $m$th
one is added, the previous $m-1$ ones won't be changed. $f$ is updated to
consist of $m$ weaker learners.

Adaboost is more specific a model in the sense that it utilized a particular
loss function called exponential loss \eqref{eq:exponentialLoss}, which in turn
leads to mathmatical convenience. Actually, it is not after five laters later
since the invention of Adaboost that people found it could be interpreted in
terms of exponential loss. Before this, people knew it worked, but it was kind
of mysterious why the mathmatical steps when adding new weak learners to the
ensemble.

\begin{equation}
    L\big(y, f(x)\big) = \exp\big(-y f(x)\big)
    \label{eq:exponentialLoss}
\end{equation}

However, exponential loss isn't ideal in terms of robustness since it penalized
mislabeled data points heavily (see [this
notebook](https://github.com/zyxue/sutton-barto-rl-exercises/blob/d2576ce4c826f2ec788bedeacb0dd8b7bd60544a/supervised/error-functions.ipynb)
for a comparison of some common loss functions). Yet, using other loss function
(e.g. binomial deviance) makes the math hairy. Then, a generalized version of
boosting was invented to make boosting works with any differential loss
function. It is called gradient boosting in the sense that when a new weak
learner (e.g. tree) is added, it is trained to fit the negative gradient of the
loss function, analogous to an update step in iterative numerical optimization.


**Reference**

I learned the difference mainly by reading the following book chapter (multiple
times, carefully)

[Chapter 10. Boosting and Additive
Trees](https://statweb.stanford.edu/~tibs/ElemStatLearn/). The Elements of
Statistical Learning: Data Mining, Inference, and Prediction. Second Edition,
February 2009, Trevor Hastie, Robert Tibshirani, and Jerome Friedman

Please let me know if you have any question or comment, and I will try to
explain better.
