---
layout: post
title:  Bash boilerplate
author: Zhuyi Xue
tags: bash
---

The following boilerplate is adopted from
[Best Practices for Writing Bash Scripts](http://kvz.io/blog/2013/11/21/bash-best-practices/)
by Kevin van Zonneveld.

{% highlight bash %}
#!/usr/bin/env bash

set -o errexit                  # equivalent to set -e
set -o nounset                  # equivalent to set -u
set -o pipefail                 # catch failure in pipe
set -o xtrace                   # equivalent to set -x

# # Set magic variables for current file & dir
# __dir="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
# __file="${__dir}/$(basename "${BASH_SOURCE[0]}")"
# __base="$(basename ${__file} .sh)"
# __root="$(cd "$(dirname "${__dir}")" && pwd)" # <-- change this as it depends on your app

# arg1="${1:-}"

{% endhighlight %}
