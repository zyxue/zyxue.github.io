---
layout: post
<!-- title:  -->
author: Zhuyi Xue
tags: mRNA-Seq, FireBrowse, FireHose, RSEM
---

Here, I summarize what I've learned about logistic regression from three sources.

**TL;DR:** 

With the posterior set to be in a logistic (sigmoid) form
(Eq. \eqref{eq:logistic}), logistic regression tries to find a
hyperplane (linear) that maximizes the likehood of the observing the
given data based on the distances from the data points to the
hyperplane.


# From [Ali Ghodsi's lecture](https://www.youtube.com/watch?v=wgCgYNM-5Cc&list=PLehuLRPyt1Hy-4ObWBK4Ab0xk97s6imfC&index=4)

The lecture introduces logistic regression by modeling the posterior
 directly.

\begin{equation}
	p(y=1|x;\beta) = \frac{e^{\beta^T x}}{1 + e^{\beta^T x}}
	\label{eq:logistic}
\end{equation}
	
Its complement is

\begin{equation}
	p(y=0|x;\beta) = 1 - p(y=1|x;\beta) = \frac{1}{1 + e^{\beta^T x}}
\end{equation}

Aggregate the two together,

\begin{equation}
	p(y|x;\beta) = (\frac{e^{\beta^T x}}{1 + e^{\beta^T x}})^y (\frac{1}{1 + e^{\beta^T x}}) ^ {1-y}
\end{equation}

Given $$x$$ and $$y$$ from the data, we want to estimate $$\beta$$ with
maximum likelihood.

\begin{equation}
    L(\beta) = \prod_{i}^{n}p(y_i|x_i,\beta)
\end{equation}

To maximize $$L$$ is equivalent to maximizing

<script type="math/tex; mode=display">% <![CDATA[
\begin{align}
	l(\beta) &= \sum_{i}^{n}\mathrm{log}(p(y_i|x_i,\beta)) \\
			 \label{eq:log-likelihood}
             &= \sum_{i}^{n} y_i[ \mathrm{log}(p(y|x;\beta))] + (1-y_i)[\mathrm{log}(1 - p(y|x;\beta))] \\
             &= \sum_{i}^{n}y_i [\mathrm{log}(e^{\beta^Tx_i}) - \mathrm{log}(1 + e^{\beta x_i}] + (1 - y_i)[- \mathrm{log}(1 + e^{\beta^Tx_i})] \\
             &= \sum_{i}^{n}y_i\beta^Tx_i - \mathrm{log}(1 + e^{\beta^T x_i})
\end{align}
%]]></script>

Taking its derivative

<script type="math/tex; mode=display">% <![CDATA[
\begin{align}
	\frac {\partial l(\beta)}{\partial \beta} &= \sum_{i}^{n}y_i x_i^T - \frac{e^{\beta^T x}}{1 + e^{\beta^T x}} x_i^T \\
                                              &= \sum_{i}^{n}y_i x_i^T - p(x_i|\beta) x_i^T
\end{align}
%]]></script>

After that, the function could be solved by [Newton-Raphson method](https://en.wikipedia.org/wiki/Newton%27s_method) (aka. Newton's method).


\begin{align}
	\frac {\partial^2 l(\beta)}{\partial \beta \partial \beta ^T} &= \sum_{i}^{n} -(1 - p(x_i | \beta) p(x_i|\beta) x_i x_i^T
\end{align}

Then updte $$\beta$$ till convergence

\begin{align}
	\beta^{\mathrm{new}} \leftarrow \beta ^{\mathrm{old}} - [\frac {\partial l(\beta)}{\partial \beta}][\frac {\partial^2 l(\beta)}{\partial \beta \partial \beta ^T}^{-1}]
\end{align}

Put [Newton-Raphson method](https://en.wikipedia.org/wiki/Newton%27s_method) in matrix form.

Define

\begin{align}
    y_{\mathrm{nx1}}
\end{align}

\begin{align}
	X_{\mathrm{n \times (d+x)}}
\end{align}

\begin{align}
	P_{\mathrm{n \times 1}} \quad \mathrm{where} \quad P_i = p(x_i | \beta)
\end{align}

and a diagonal matrix
\begin{align}
    W \quad \mathrm{where} \quad W_{ii} = (1 - p(x_i|\beta))(p(x_i|\beta))
\end{align}


Then, 

\begin{align}
	\frac {\partial l(\beta)}{\partial \beta} = (y - P) ^T X
\end{align}

\begin{align}
	\frac {\partial^2 l(\beta)}{\partial \beta \partial \beta ^T} = -X^T W X
\end{align}

<span style='color:red'>Still need to check the dimension of matrix multiplication!</span>

\begin{align}
	\beta^{\mathrm{new}} \leftarrow \beta ^{\mathrm{old}} - ((y - P) ^T X)(-X^T W X)^{-1}
\end{align}



# From [Andrew Ng's course](https://www.coursera.org/learn/machine-learning/home/week/3)

Also summarized [here](http://ufldl.stanford.edu/tutorial/supervised/LogisticRegression/).

Posterior is set to be 

\begin{equation}
    p(y=1|x;\beta) = \frac{1}{1 + e^{\beta^T x}}
\end{equation}

Logistic regression is solved by minimizing a cost function (log loss)

\begin{equation}
    J(\beta) = - \sum_{i}^{n} y_i \mathrm{log} (p(y_i|x_i,\beta)) + (1 - y_i) \mathrm{log} (1 - p(y_i|x_i, \beta))
\end{equation}

or simply

\begin{equation}
    J(\beta) = - \sum y \mathrm{log} (p) + (1 - y) \mathrm{log} (1 - p)
	\label{eq:logloss}
\end{equation}

and $$J(\beta)$$ could be minimized with gradient descent.

This is exactly the same as the log-likelihood, comparing
Eq.\eqref{eq:logloss} to Eq.\eqref{eq:log-likelihood}, but the minus
sign at the beginning. The minus sign is also the reason why the
log-likelihood needs maximized while the log loss needs minimized.


# From [sklearn](http://scikit-learn.org/stable/modules/generated/sklearn.linear_model.LogisticRegression.html)

Instead of Newton-Raphson method and gradient descent, sklearn
provides alternatives.

1. newton-cg: [Newton conjugate gradient method](https://en.wikipedia.org/wiki/Nonlinear_conjugate_gradient_method)
1. sag: [Stochastic gradient descent](http://scikit-learn.org/stable/modules/sgd.html)
1. lbfgs: [Limited-memory BFGS](https://en.wikipedia.org/wiki/Limited-memory_BFGS)
1. [liblinear](https://www.csie.ntu.edu.tw/~cjlin/liblinear/)
1. [SAGA](https://arxiv.org/abs/1407.0202)

Different solvers have different limitations when it comes to what regularization methods (e.g. L1, L2) are applicable.

I am not an expert on numerical optimization.
