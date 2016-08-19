---
layout: post
title:  Sort text numberically with Linux sort
author: Zhuyi Xue
tags: Linux,sort
---

**Problem**:

Sort the following items in `input.txt` by the number in them.

<pre>
a_100
b_101
c_102
d_23
e_24
f_25
</pre>

Expected output:

<pre>
d_23
e_24
f_25
a_100
b_101
c_102
</pre>

**Solution**: `sort -t '_' -k 2n input.txt`

**Explanation**: 

* `-t '_'`: set the delimiter to the underscore character
* `-k 2n`: sorts by the second column using numeric ordering

Reference: [http://stackoverflow.com/questions/13088370/sort-numerically](See http://stackoverflow.com/questions/13088370/sort-numerically)
