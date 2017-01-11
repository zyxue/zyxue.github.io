---
layout: post
title: Javascript prototype exploration
author: Zhuyi Xue
tags: Python
---


Inspired from *Inheritance and the prototype chain* on
this
[page](https://developer.mozilla.org/en/docs/Web/JavaScript/Inheritance_and_the_prototype_chain),
I decided to figure out prototype chains of all Javascript primitive types.


There
are
[6 primitive types](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Data_structures) inlcuding
`null` and `undefined`, plus `Object`. However, `null` does not have a prototype
chain as it is the root of the chain already, and `undefined` does not have
protype chain, either.

First we defined a function to print the chain. Then, apply it to all remaining 4
primitive types and `Object`.

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

Now, we test it out

{% highlight javascript %}
[Boolean, Number, String, Symbol, Object].forEach((type) => {
    console.log(getPrototypeChains(type));
    console.log('---');  // separator
});
{% endhighlight %}

Output:

{% highlight default %}
(function Boolean() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Number() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function String() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Symbol() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
(function Object() { [native code] }, function) => (function () {}, function) => ([object Object], object) => (null, object)
---
{% endhighlight %}

So basically, every primitive type is a specialized function that inherits the
`function` function, which inherits the `object Object`, which inherits from
`null`.


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

Look more carefully, and see what they are. You could inspect each node in the
browser dev tools, which will provide more insight as they won't be printed just
in plain string.
