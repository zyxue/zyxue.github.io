---
layout: post
title: YAML newline syntax
author: Zhuyi Xue
tags: yaml
---

A note about the different newline syntax in
[YAML](https://docs.ansible.com/ansible/latest/reference_appendices/YAMLSyntax.html).

Suppose `test.yaml` has the following content:

{% highlight yaml %}
a: |
  z
  w

b: >
  z
  w

c: >-
  z
  w
{% endhighlight %}

Loading it with [PyYAML](https://pyyaml.org/):

```
>>> import yaml
>>> with open('./test.yaml') as opened:
...     yaml.safe_load(opened)
... 
{'a': 'z\nw\n', 'b': 'z w\n', 'c': 'z w'}
```

So

* `|` keeps all newline characters (`\n`).
* `>` only keeps the last newline character.
* `>-` removes all newline characters.
