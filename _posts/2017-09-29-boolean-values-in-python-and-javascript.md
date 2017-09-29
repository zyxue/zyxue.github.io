---
layout: post
<!-- title:   -->
author: Zhuyi Xue
tags: genomics, alignment, SAM format
---

The boolean evaluations of different values in different languages:

## Python

ref: https://docs.python.org/3/library/stdtypes.html#truth-value-testing

Values evaluated to be false.

1. Constants defined to be false: `None` and `False`.
1. Zero of any numeric type: `0`, `0.0`, `0j`, `Decimal(0)`, `Fraction(0, 1)`
1. Empty sequences and collections: `''`, `()`, `[]`, `{}`, `set()`, `range(0)`


## Javascript

ref: https://github.com/airbnb/javascript#comparison-operators--equality

1. Objects evaluate to true
1. `Undefined` evaluates to false
1. `Null` evaluates to false
1. Booleans evaluate to the value of the boolean
1. Numbers evaluate to false if `+0`, `-0`, or `NaN`, otherwise true
1. Strings evaluate to false if an empty string `''`, otherwise true
