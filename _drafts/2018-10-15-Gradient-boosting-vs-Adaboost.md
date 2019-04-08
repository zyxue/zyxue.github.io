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


**tl;nr**: Adaboost can be formulated as gradient boosting for classification
with {1, -1} encoding of positive and negative labels and a specific loss
function: exponential loss (see Eq. \eqref{eq:exponentialLoss}).


Jerome H. Friedman propsed Gradient boosting in 2000? ([ref]), which is an
additive model that adds a new estimator (e.g. a decision tree) to fit residual
error from previous steps one by one.

\begin{equation}
    f(x) = \sum_{m=1}^{M} \beta_{m}b(x;\gamma_m)
    \label{eq:additiveModel}
\end{equation}

where 

* $$\beta_m$$ is the associated coefficent (also named expansion coefficient in
the aforementioned book chapter), and
* $$b(x;\gamma_m$$) is the $$m$$th basis function (e.g. tree stump) with
  * $$x$$ being the input data, and 
  * $$\gamma_m$$ being the model parameters (e.g. split variable and cutoff for a
    tree stump).


It is propsed after the invention of
Adaboost, and from statistical perspective, Friedman Adaboost could be
formulated as gradient boosting with a specific form of loss function,
expoential loss.

\begin{equation}
    L(y, f(x)) = e^{(-y f(x))}
    \label{eq:exponentialLoss}
\end{equation}


Compared to another common model, random forest (RF), gradient boosting is
additive while RF use bagging. As result, in GB, the later estimator depends on
the performance of the previous estimator, while in RF, the multiple estimators
are independent of each other, hence they could be trained in parallel by
fitting a subset of the features or samples, or boths (to confirm).
