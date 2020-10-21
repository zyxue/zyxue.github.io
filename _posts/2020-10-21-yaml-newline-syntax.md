---
layout: post
title: YAML newline syntax
author: Zhuyi Xue
tags: yaml
---

To keep a note about the different newline syntax in YAML

{% highlight yaml %}
```
a: |
  z
  w

b: >
  z
  w

c: >-
  z
  w
```
{% end yaml %}

would become

```
{'a': 'z\nw\n', 'b': 'z w\n', 'c': 'z w'}
```

after loaded with `yaml.safe_load` in Python.


So

* `|` keeps all newline characters (`\n`).
* `>` only keeps the last newline character.
* `>-` removes all newline characters.
