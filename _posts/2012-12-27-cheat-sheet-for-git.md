---
layout: post
title:  Cheat sheet for Git
author: Zhuyi Xue
tags: cheat sheet,git
---


#### Most commonly used commands:

{% highlight bash %}
git remote add origin git@github.com:someaccount/something.git

git pull origin master
git pull origin an_other_branch
git push origin master
git remote show origin
git remote -v
git branch an_other_branch
git branch -d an_other_branch
git commit -m "some description about the commit"
git checkout an_other_branch
git checkout master
git merge an_other_branch
{% endhighlight %}

#### Remove file from index ONLY
{% highlight bash %}
git rm --cached
{% endhighlight %}

#### See what changes will be made in the next commit
{% highlight bash %}
git diff -cached
{% endhighlight %}

#### View changes at each commit
{% highlight bash %}
git log -p
{% endhighlight %}

#### Get an overvie of the changes in each
{% highlight bash %}
git log --stat --summary
{% endhighlight %}

#### List both remote-tracking branches and local branches.
{% highlight bash %}
git branch -a
{% endhighlight %}

{% highlight bash %}
git log -p HEAD FETCH_HEAD
{% endhighlight %}

#### Clean up git repostitory
{% highlight bash %}
git gc --aggressive
{% endhighlight %}

#### Remove file from tracking without deleting
{% highlight bash %}
git rm -r --cached to_be_ignored
{% endhighlight %}

{% highlight bash %}
tar zxvf run.tgz --strip-components 3 --show-transformed
{% endhighlight %}
