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
primitive types and `Object`. Here is what it looks like



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

[Boolean, Number, String, Symbol, Object].forEach((type) => {
    console.log(getPrototypeChains(type));
    console.log('---');  // separator
});
{% endhighlight %}

Output:


```
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
```

So basically, every primitive type is a specialized function that inherits the
`function` function, which inherits the `object Object`, which inherits from
`null`.
