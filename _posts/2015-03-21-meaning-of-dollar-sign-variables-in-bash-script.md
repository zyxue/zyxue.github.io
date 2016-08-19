---
layout: post
title:  Meaning of dollar sign variables in bash script (with examples)
author: Zhuyi Xue
tags: bash, dollar sign variables
---

* `$1`, `$2`, `$3`, ... are the positional parameters. e.g.

{% highlight bash %}
# cat ./script.sh
echo first param: $1
echo second param: $2
echo third param: $3

# bash ./script.sh a b c
# first param: a
# second param: b
# third param: c
{% endhighlight %}

* `$@`: an array-like construct of all positional parameters, {`$1`, `$2`, `$3`,
  ...}. e.g.

{% highlight bash %}
# cat ./script.sh
echo $@
    
# bash ./script.sh a b c
# a b c
{% endhighlight %}

* `$*` is the IFS (Internal Field Separator) expansion of all positional
  parameters, `$1`, `$2`, `$3`, .... It looks similar to but is still different
  from `$@` depending on the special shell variable `$IFS` and whether `$*` is
  double quoted or not. See [here](https://bash.cyberciti.biz/guide/$IFS) for more
  details. e.g.

{% highlight bash %}
# cat ./script.sh
echo $*
echo "$*"
IFS='|'
echo $*
echo "$*"
IFS='#'
echo "$*"

# bash ./script.sh a b c
# a b c
# a b c
# a b c
# a|b|c
# a#b#c
{% endhighlight %}

* `$#` is the number of positional parameters. e.g.

{% highlight bash %}
# cat ./script.sh
echo $#

# bash ./script.sh a b c
# 3
{% endhighlight %}

* `$-` current options set for the shell. See this [post]({% post_url
  2016-04-23-bash-boilerplate %} ) for some common shell options available, and
  this
  [post](https://www.chainsawonatireswing.com/2012/02/02/find-out-what-your-unix-shells-flags-are-then-change-them//?from=@)
  for more details on `$-`. e.g.

{% highlight bash %}
# cat ./script.sh
set -e
echo $-

# bash ./script.sh
# eHb
# Hb appears to be default.
{% endhighlight %}

* $$ pid of the current shell (not subshell)

{% highlight bash %}
# cat ./script.sh
echo $$

# bash ./script.sh
# 71061
# can be some other number, as well.
{% endhighlight %}

* $_ most recent parameter (or the absolute path of the command to start the
 current shell immediately after startup)

{% highlight bash %}
# cat ./script.sh
echo $_

# ls -l
# bash ./script.sh
# -l
# because -l is the most recent parameter used, I don't know why this is useful?!
{% endhighlight %}

* `$IFS` the (input) field separator, see how it's used together with `$*` as
  described above

* `$?` most recent foreground pipeline exit status.

* `$!` PID of the most recent background command

* `$0` name of the shell or shell script

{% highlight bash %}
# cat this_awesome_script.sh
echo $0

# $bash this_awesome_script.sh
# this_awesome_script.sh
{% endhighlight %}

Reference: http://stackoverflow.com/questions/5163144/what-are-the-special-dollar-sign-shell-variables
