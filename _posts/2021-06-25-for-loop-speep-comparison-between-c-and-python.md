---
layout: post
title: Comparison of for loop speed between C and Python
author: Zhuyi Xue
tags: operating system, process
---

A concrete example of C is much faster than plain Python:

# Python:

{% highlight python %}
total = 0

for i in range(int(1e8) + 1):
    total += i

print(total)
{% endhighlight %}

{% highlight bash %}
time python for_loop.py 
5000000050000000
python for_loop.py  9.06s user 0.04s system 99% cpu 9.114 total
{% endhighlight %}

# C:

{% highlight c %}
#include <stdio.h>

int main()
{
    int i;
    long total = 0;

    for (i = 0; i <= (int)1e9; i++)
    {
        total += i;
    }

    printf("%ld\n", total);
}
{% endhighlight %}

{% highlight bash %}
gcc for_loop.c -o for_loop
time ./for_loop
500000000500000000
./for_loop  1.36s user 0.00s system 81% cpu 1.673 total
{% endhighlight %}

So in this simple example, C is about 5.5x (9.114 / 1.673) faster than Python.
