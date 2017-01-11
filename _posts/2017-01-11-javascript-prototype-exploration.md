---
layout: post
title: Javascript prototype exploration
author: Zhuyi Xue
tags: Python
---


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
