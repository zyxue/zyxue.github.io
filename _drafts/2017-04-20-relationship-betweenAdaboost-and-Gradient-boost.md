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

TL;DR:

The two are not concepts at the same level, hence not so meaningful for
comparison. Instead, the higher level concept is **forward stagewise additive
modeling (FSAM)**. 

$$f_m(x) = f_{m-1}(x) + \beta_m b(x;\gamma_m)$$

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

$$f(x) = \sum_{m=1}^{M} \beta_{m}b(x;\gamma_m)$$

where $$\beta_m$$ are coefficients assigned to each individual model
$$b(x;\gamma_m$$) output.

The sepcialty about FASM is that there is an element of sequence dependency in
it, hence forward stagewise, i.e. at iteration $$m$$, $$f_m$$ depends on
$$f_{m-1}$$, and the parameters of the previously added basis functions won't be
adjusted in later iterations. The advantage of such dependency is that it makes
the model optimization (learning) process easier. I have no clue how to optimize
the additive model altogether at once, considering hundreds of thousands of
$$b$$.

Now, we get to **Adaboost**, which is simply a specific case of FSAM, with an
exponential loss function:

$$L(y, f(x)) = exp(-y f(x))$$.

asd

\begin{equation}
   E = mc^2
   \label{emc}
\end{equation}


This is very generic a name, FYI, other common loss functions include accuray,
binomial deviance (aka. cross-entroy), and squared error. Figure 10.4 in the
book chapter has a comparison of them, and it explains them well. It proved that
with such a loss function, Adaboost, fits an FSAM. It can be induced to a form
consistent compatible with Equation \eqref{emc}.

Let's consider <math><mi>a</mi><mo>≠</mo><mn>0</mn></math>.
       

Inspired from *Inheritance and the prototype chain* on
this
[page](https://developer.mozilla.org/en/docs/Web/JavaScript/Inheritance_and_the_prototype_chain),
I decided to figure out prototype chains of all standard built-in objects in Javascript.

First we defined a function to print the chain.

{% highlight javascript %}
function formatNode(o) {
    // specifiy how a node should be formatted in the chain
    return '(' + o + ', ' + typeof o + ')';
}

function getPrototypeChains(type) {
    let o = type;
    let chains = [];
    chains.push(formatNode(o));
    while (o !== null) {
        o = Object.getPrototypeOf(o);
        chains.push(formatNode(o));
    }
    return chains.join(' => ');
}
{% endhighlight %}

Now, we test it out for all the standard built-in objects collected from [here](https://developer.mozilla.org/en-US/docs/Web/JavaScript).

{% highlight javascript %}
[Array,
 Boolean,
 Date,
 Error,
 Function,
 JSON,
 Math,
 Number,
 Object,
 RegExp,
 String,
 Map,
 Set,
 WeakMap,
 WeakSet].forEach((type) => {
     console.log(getPrototypeChains(type));
     console.log('---');  // separator
 });
{% endhighlight %}

Output:

{% highlight default %}
(function Array() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Boolean() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Date() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Error() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Function() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
([object JSON], object) => ([object Object], object) => (null, object)
---
([object Math], object) => ([object Object], object) => (null, object)
---
(function Number() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Object() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function RegExp() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function String() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Map() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Set() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function WeakMap() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function WeakSet() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
{% endhighlight %}

As you can see, most of them are specialized functions that inherit from the
`function` function, which inherits from the `object Object`, which inherits
from `null`.

`JSON` and `Math` are the exceptions as they inherit from the `object Object`
directly. 

Also, it shows that `typeof null`is an object.

Let's try it with some real values

{% highlight javascript %}
[true, 1, 'xyz',
 // Symbol would create trouble when converting to string, so commented out for
 // now, you're encouraged to try it out, and see what happens
 // Symbol('s'),
 {'a': 1}
].forEach((type) => {
    console.log(getPrototypeChains(type));
    console.log('---');  // separator
});
{% endhighlight %}

Output:

{% highlight default %}
(true, boolean) => (false, object) => ([object Object], object) => (null, object)
---
(1, number) => (0, object) => ([object Object], object) => (null, object)
---
(xyz, string) => (, object) => ([object Object], object) => (null, object)
---
([object Object], object) => ([object Object], object) => (null, object)
---
{% endhighlight %}

Look more carefully, and see what they are. You could also inspect each node in
the browser dev tools, which will provide more insight as they won't be have to
be printed in just plain string.
