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
set -o xtrace                   # equivalent to set -x
set -o pipefail                 # catch failure in pipe

# Backup stuffs:

# # Set magic variables for current file & dir
# __dir="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"
# __file="${__dir}/$(basename "${BASH_SOURCE[0]}")"
# __base="$(basename ${__file} .sh)"
# __root="$(cd "$(dirname "${__dir}")" && pwd)" # <-- change this as it depends on your app

# arg1="${1:-}"

{% endhighlight %}


`set -e` exits the execution upon a command failure. E.g. for a script like 
```
set -e

ls abc

pwd
```

The error is like
```
ls: cannot access 'abc': No such file or directory
```

Note, the line `pwd` is never executed.

`set -u` exits the execution when a unbound variable is encoutered. E.g. for a script like

```
set -u

echo $ABC
```

the error is like
```
line 3: ABC: unbound variable
```

`set -x` shows the trace of execution. E.g. for a script like

```
set -x

echo 1

echo 2
```

the output is
```
+ echo 1
1
+ echo 2
2
```

`set -o pipefail` returns the non-zero status code of failed command in a pipeline. E.g. for 
```
# set -o pipefail

ls non-existing-file | wc

echo $?
```

the output is `0`, the return code of `wc`; but if we uncomment the first
line. the output is 2, i.e. the return code of `ls non-existing-file`.


