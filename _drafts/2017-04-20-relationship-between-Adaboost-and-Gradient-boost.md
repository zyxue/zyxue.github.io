---
layout: post
author: Zhuyi Xue
tags: machine learning, boosting
---

<script type="text/x-mathjax-config">
MathJax.Hub.Config({
  TeX: { equationNumbers: { autoNumber: "AMS" } }
  
});
</script>

<p style="color:red; font-size:30px;"><strong>UPDATE:</strong> this post has
been updated at <a
href="/2018/10/15/Gradient-boosting-vs-Adaboost">here</a>.</p>


**Q**: What's the difference between [Adaboost](https://en.wikipedia.org/wiki/AdaBoost)
and [Gradient boosting](https://en.wikipedia.org/wiki/Gradient_boosting)?

I'd like to share my current understanding of this question after some reading:

1. Friedman, Jerome; Hastie, Trevor; Tibshirani, Robert. Additive logistic regression: a statistical view of boosting (With discussion and a rejoinder by the authors). Ann. Statist. 28 (2000), no. 2, 337--407. doi:10.1214/aos/1016218223. [http://projecteuclid.org/euclid.aos/1016218223](http://projecteuclid.org/euclid.aos/1016218223).
1. Friedman, Jerome H. Greedy function approximation: A gradient boosting machine. Ann. Statist. 29 (2001), no. 5, 1189--1232. doi:10.1214/aos/1013203451. [http://projecteuclid.org/euclid.aos/1013203451](http://projecteuclid.org/euclid.aos/1013203451).
1. [Chapter 10. Boosting and Additive Trees](https://statweb.stanford.edu/~tibs/ElemStatLearn/). The Elements of Statistical Learning: Data Mining, Inference, and Prediction. Second Edition, February 2009, Trevor Hastie, Robert Tibshirani, and Jerome Friedman

They are all from the same authors. The first two are very technically
detailed, while the book chapter provides a relatively more high-level view.

**TL;DR:**

1. The two are concepts not at the same level, hence their comparison are not so meaningful.
2. Two more higher level concepts are needed to explain the relationship between
   them: 
   1. Forward stagewise additive modeling (FSAM)
   1. Additive model

Basically, 

1. FSAM is a specific form of additive model
1. Adaboost is a specific form of FSAM
1. Gradient boosting, on the other hand, is a numerical process for optimizing
   FSAM using gradient descent by treating functions as numerical parameters.
1. Adaboost, given it is a two-class classification problem with scaled
   classification trees (with $$-1/1$$ encoding for classes), and a specific
   loss function (exponent loss), the opimization of its coressponding FSAM is
   relatively easy even without gradient descent.

**More:**

Let's start with **additive modeling** first, which is effectively the
ensemble method, i.e. combining how multiple models ($$M$$) together.

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

**Forward stagewise additive modeling (FSAM)** is also additive,

\begin{equation}
    f_m(x) = f_{m-1}(x) + \beta b(x;\gamma)
    \label{eq:fsam}
\end{equation}

where $$f_m(x)$$ is the learned model at the $$m$$th iteration (e.g. boosting
iteration), and there could be $$M$$ iterations.

However, its sepcialty is that there is an element of sequence dependency in it,
hence the so-called forward stagewise, i.e. at iteration $$m$$, $$f_m$$ depends
on $$f_{m-1}$$. Noteably, the parameters of the previously added basis functions
won't be adjusted in later iterations. The advantage of such a dependency is
that it makes the model optimization (learning) process easier. I still have no
clue how to optimize the additive model \eqref{eq:additiveModel} altogether at
once, considering hundreds of thousands of $$b$$ (e.g. $$M=1000$$).

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
optimizing FSAM using gradient descent. 

TODO: will update with more details on **gradient boosting** later.


Thank you for reading along. Please leavage a message and correct me if my
understanding is not correct.

<!-- (This is a tricky part that I still don't understand well, how to do -->
<!-- optimization on the function space?) -->

<!-- This is very generic a name, FYI, other common loss functions include accuray, -->
<!-- binomial deviance (aka. cross-entroy), and squared error. Figure 10.4 in the -->
<!-- book chapter has a comparison of them, and it explains -->
