---
layout: post
author: Zhuyi Xue
tags: Python
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
  
});
</script>

<script type="text/javascript"
     src="https://d3eoax9i5htok0.cloudfront.net/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML">
</script>

**Q**: What's the difference between [Adaboost](https://en.wikipedia.org/wiki/AdaBoost)
and [Gradient boosting](https://en.wikipedia.org/wiki/Gradient_boosting)?

I have been pondering on this question for a while now. Having searched
for posts and wiki, I found them still not so helpful for my understanding. So I
digged a bit deeper. I read the two papers and a related book chapter:

1. Friedman, Jerome; Hastie, Trevor; Tibshirani, Robert. Additive logistic regression: a statistical view of boosting (With discussion and a rejoinder by the authors). Ann. Statist. 28 (2000), no. 2, 337--407. doi:10.1214/aos/1016218223. [http://projecteuclid.org/euclid.aos/1016218223](http://projecteuclid.org/euclid.aos/1016218223).
1. Friedman, Jerome H. Greedy function approximation: A gradient boosting machine. Ann. Statist. 29 (2001), no. 5, 1189--1232. doi:10.1214/aos/1013203451. [http://projecteuclid.org/euclid.aos/1013203451](http://projecteuclid.org/euclid.aos/1013203451).
1. [Chapter 10. Boosting and Additive Trees](https://statweb.stanford.edu/~tibs/ElemStatLearn/). The Elements of Statistical Learning: Data Mining, Inference, and Prediction. Second Edition, February 2009, Trevor Hastie, Robert Tibshirani, and Jerome Friedman

Not until I finished reading, did I realize they were all from the same authors.
The first two are really technically detailed, but it's the book chapter that
really helped me see the overall picture. So here, I am trying to summarize and
share my current understanding of this question.

_TL;DR: TODO_

The two are not concepts at the same level, hence not so meaningful for
comparison. Instead, the higher level concept is **forward stagewise additive
modeling (FSAM)**. 

\begin{equation}
    f_m(x) = f_{m-1}(x) + \beta_m b(x;\gamma_m)
    \label{eq:fsam}
\end{equation}

where

1. $$f_m(x)$$ is the learned model at the $$m$$th iteration (e.g. boosting
iteration),
1. $$b$$ is the basis function (e.g. tree stump),
1. $$\beta$$ is the associated coefficent (also named expansion coefficient in
the aforementioned book chapter).
1. $$x$$ is the data, and
1. $$\gamma$$ is the model parameters (e.g. split
variable and cutoff if $$b$$ is a tree stump).

An even higher level concept is **additive modeling**, which is effectively the
ensemble method, i.e. combining how multiple models together.

\begin{equation}
    f(x) = \sum_{m=1}^{M} \beta_{m}b(x;\gamma_m)
    \label{eq:additiveModel}
\end{equation}

where $$\beta_m$$ are coefficients assigned to each individual model
$$b(x;\gamma_m$$) output.

The sepcialty about FASM is that there is an element of sequence dependency in
it, hence the so-called forward stagewise, i.e. at iteration $$m$$, $$f_m$$
depends on $$f_{m-1}$$. Noteably, the parameters of the previously added basis
functions won't be adjusted in later iterations. The advantage of such a
dependency is that it makes the model optimization (learning) process easier. I
still have no clue how to optimize the additive model \eqref{eq:additiveModel}
altogether at once, considering hundreds of thousands of $$b$$ (e.g.
$$M=1000$$).

Now, we get to **AdaBoost**, which is a specific case of FSAM, with an
exponential loss function:

\begin{equation}
    L(y, f(x)) = e^{(-y f(x))}
    \label{eq:exponentialLoss}
\end{equation}

The is not obvious at all without looking at the algorithm of Adaboost. But even
after so, it's not so intuitive. That **Adaboost* is a type of FSAM was not
discovered till 5 years later it was first invented. After all, it's proved in
the book chapter (I also rederived it on paper) that by plugging in
\eqref{eq:exponentialLoss}, adaboost fits into a specific case of
\eqref{eq:fsam}.

Before we get to gradient boosting, let's look at the exponent of the loss
function $$(y f(x))$$ with negative sign removed. This is called the "margin",
and interestingly it has a analogous role to the residual $$y - f(x)$$ in
regression. For formalize it a bit the two expressions are analogous:

* $$y \cdot f(x)$$ in a classification problem with ($$-1/1$$ response)
* $$y - f(x)$$ in a regression problem: 

where $$y$$ is the truth label and $$f(x)$$ is the model prediction. Concretely,
when the $$y f(x)$$ is $$1$$ when the classification is correct otherwise
$$-1$$.

Now, let's get to **gradient boosting**, which describes a numerical method for
optimizing FSAM in general.

(This is a tricky part that I still don't understand well, how to do
optimization on the function space?)

<!-- This is very generic a name, FYI, other common loss functions include accuray, -->
<!-- binomial deviance (aka. cross-entroy), and squared error. Figure 10.4 in the -->
<!-- book chapter has a comparison of them, and it explains -->
