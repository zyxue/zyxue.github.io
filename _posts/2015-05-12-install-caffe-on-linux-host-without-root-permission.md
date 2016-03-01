---
layout: post
title:  Install caffe on linux host without root permission
author: Zhuyi Xue
tags:  neural networks,caffe,linux,linuxbrew,anaconda
---

Here, I'd like to share my experience of installing the popular deep learning
framwork,
[Caffe](http://caffe.berkeleyvision.org/gathered/examples/mnist.html), on a
linux host without root permission. Here, it mainly covers the GPU mode
installation. The CPU mode is easier, and should be easily accomplished with
simple variation from the steps described below.

**Prerequisites**: [CUDA](http://www.nvidia.ca/object/cuda_home_new.html) has
to be installed already since it needs root permission, but that's the only
one.

General guide, try to use

* [linuxbrew](https://www.blogger.com/git@github.com:Homebrew/linuxbrew.git)
  for installing C/C++ packages, and
* [anaconda](https://store.continuum.io/cshop/anaconda/) for installing python
  packages.
* I don't use
  [matlab](https://www.google.ca/webhp?sourceid=chrome-instant&ion=1&espv=2&ie=UTF-8#q=matlab),
  so there won't be mention on matlab related packages
* Try to avoid installing packages mannually, it's not fun and costs a lot of
  time. But if you have never installed packages and managed dependencies
  yourself, it can be a good exercise.

Let's begin to follow the instructions on the
[Caffe installation page](http://caffe.berkeleyvision.org/installation.html). From
here on, I assume you've successfully installed linuxbrew and anaconda.

First, let's get linuxbrew up-to-date.

{% highlight bash %}
$ brew update
{% endhighlight %}

Then we start installing the first package, BLAS. At the time of writing, only
[OpenBLAS](http://www.openblas.net/) is available in linuxbrew, so try

{% highlight bash %}
# -dv means debug mode and verbose
$ brew install -dv openblas
{% endhighlight %}

If the above command complains about [gfortran](https://gcc.gnu.org/fortran/)
optimization problem, you may want to look closer to see why it complains or
seek for a openblas already installed by system admin. I am not sure how the
optimization problem would affect caffe's performance, but since caffe is a
piece of speed-critical software, I think it would be worth looking into it.

Next [boost](http://www.boost.org/),

{% highlight bash %}
$ brew install -dv boost
{% endhighlight %}

Here is a tricky one, compiling caffe needs `-lboost-thread`, but the boost
installed by linuxbrew only have `libboost-thread-mt.a` and
`libboost-thread-mt.so` available, and caffe will complain later. My fix is to
do the following symlinks

{% highlight bash linenos %}
$ cd ~/.linuxbrew/Cellar/boost/1.58.0/lib/
$ ln -s libboost_thread-mt.a libboost_thread.a
$ ln -s libboost_thread-mt.so libboost_thread.so
# you need to relink boost, and you should see the two additional files linked
# from the output of linuxbrew
$ brew unlink boost && brew link boost
{% endhighlight %}

Here I assumed the one with `-mt`, which means multiple-threading by the way,
and the one without it are the same, partially based on
[this thread](http://stackoverflow.com/questions/3031768/boost-thread-linking-boost-thread-vs-boost-thread-mt).


Next [OpenCV](http://opencv.org/),

{% highlight bash linenos %}
# This is the info after I have installed OpenCV, yours is probably DIFFERENT.
$ brew info opencv
opencv: stable 2.4.11, devel 3.0.0-rc1, HEAD
http://opencv.org/
/home/zyxue/.linuxbrew_parallel/Cellar/opencv/2.4.11_1 (223 files, 33M) *
Built from source with: --without-numpy, --without-eigen, --without-openexr
From: https://github.com/homebrew/homebrew-science/blob/master/opencv.rb
==> Dependencies
Build: cmake ✔, pkg-config ✔
Required: jpeg ✔, libpng ✔, libtiff ✔
Recommended: eigen ✘, openexr ✘, homebrew/python/numpy ✘
Optional: gstreamer ✘, jasper ✘, libdc1394 ✘, openni ✘, qt ✘, tbb ✘, ffmpeg ✘
{% endhighlight %}

Based on info, you could see OpenCV has quite a number of dependencies. I have
tried the same process on different hosts, sometimes the recommended packages
also cause headaches because they require further dependencies which may or may
not cause extra compiling problems. As a compromise, I chose to be a
minimalist. Since here what we care is mainly to install caffe, not OpenCV
itself, so I disable all the recommended (using --without-something) and
optional ones (disabled by default). The result command is

{% highlight bash %}
$ brew install -dv opencv --without-eigen --without-openexr  --without-numpy
{% endhighlight %}

Next [protobuf](https://developers.google.com/protocol-buffers/),

{% highlight bash %}
$ brew install -dv protobuf
{% endhighlight %}

This is an easy one, and I didn't encounter any issue with it. Next I installed
[gflags](http://gflags.github.io/gflags/) first before
[glog](https://github.com/google/glog) because gflags is a dependency of glog,
and its installation needs some tweaking in interactive mode.

{% highlight bash linenos %}
# to enter interactive mode, use -i.
# In the interactive mode, a sandbox is created by linuxbrew with relevant
# environment variables setup.
$ brew install -i gflags
# In the interactive mode, this is what I did
# pwd would be something like
$ pwd
/tmp/gflags20150512-19244-12eed5c/gflags-2.1.2
$ create prefix dir
mkdir -p ~/.linuxbrew/Cellar/gflags/2.1.2
$ make build
$ cd build
# create CMakeCache.txt
$ cmake -DCMAKE_INSTALL_PREFIX:PATH=~/.linuxbrew/Cellar/gflags/2.1.2 ..
# Now use you favorite editor to make the following change by appending -fPIC
# option to CMAKE_CXX_FLAGS:STRING in the CMakeCache.txt file. For example,
# CMAKE_CXX_FLAGS:STRING='-Os -w -pipe -march=core2 '
# becomes
# CMAKE_CXX_FLAGS:STRING='-Os -w -pipe -march=core2 -fPIC'
$ cmake ..
$ make
$ make install
$ exit
{% endhighlight %}

If you don't use `-fPIC`, later on when installing glog, it will complain with
the following kind of error.

{% highlight bash %}
relocation R_X86_64_32S against `.rodata' can not be used when making a shared object; recompile with -fPIC
{% endhighlight %}

Now if you have followed the above and installed gflags with `-fPIC`, glog
installation should proceed successfully.

{% highlight bash %}
$ brew install -dv glog
{% endhighlight %}

Then [hdf5](https://www.hdfgroup.org/HDF5/),
[leveldb](https://github.com/google/leveldb),
[snappy](http://google.github.io/snappy/), [lmdb](http://symas.com/mdb/),

{% highlight bash %}
$ brew install -dv hdf5 leveldb snappy lmdb
{% endhighlight %}

I find the above four relatively easy to install and no complain. Now for the
python packages. All the packages required by pycaffe are in anaconda except
for leveldb, so we seek pip for help.

{% highlight bash %}
$ conda install Cython numpy scipy scikit-image matplotlib ipython \
    h5py networkx nose pandas python-dateutil protobuf python-gflags \
    pyyaml Pillow
$ pip install leveldb
{% endhighlight %}

It seems that conda and pip install packages into the same directory
(i.e. `~/anaconda/lib/python2.7/site-packages/`), so I think when a conda
package is not available, then use pip (Please correct me if I am wrong, or you
can checkout [Anaconda Cloud](https://anaconda.org/)). Or maybe you want to
create an isolated environment with `conda create` first, which should be OK,
too. I like Anaconda because it handles packages like scipy very well, which
can be diffcult to compile from source on some linux setup.

Next, as for [cuDNN](https://developer.nvidia.com/cudnn), I am unable to use it
because it seems to require a CUDA dirver of version higher than
6.5. Otherwise, error pops out.  Unfortunately, what I have is only 6.0, so I
have to skip it.

Now we're ready install caffe, Yey! Everything should be straight forward from
now on except for one thing, which will be told soon. Just follow the steps on
the caffe page.

{% highlight bash linenos %}
$ cp Makefile.config.example Makefile.config
# Adjust Makefile.config, if you've followed the above steps exactly,
# Make.config should be easy to follow.
# The include and lib files for linuxbrew-installed packages are usually in
# ~/.linuxbrew/include and
# ~/.linuxbrew/lib
# If you're curious enough to take a
# look inside, you'll find they're mostly symlinks to the directory 
# where the packages are installed
$ make all
$ make test
$ make runtest
# after all tests pass
$ make pytest
$ make distribute
{% endhighlight %}

One final bug I encountered on one of the hosts I tried is that when making
pytest is that it complains `libboost_python.so` is not available.  After some
search, it turns out that it can be installed with

{% highlight bash %}
brew install -dv boost-python
{% endhighlight %}

However, at the time of writing, the formula for boost-python seems to be
cooked for Mac only. This isn't a surprise since linuxbrew is forked from
homebrew, which was initially designed for Mac OS X. Anyway, here is my fix

{% highlight bash linenos %}
# Open the formula in your editor
$ brew edit boost-python
# change the following line
# file.write "using darwin : : #{ENV.cxx} ;\n"
# to
# file.write "using gcc : [version number e.g. 4.4] : #{ENV.cxx} ;\n"
{% endhighlight %}

Without this modification, Mac OS X related error would happen, and you're
encouraged to take a look and see how it looks. The modification above is based
on the format of
[user-config.jam](http://www.boost.org/build/doc/html/bbv2/overview/configuration.html),
I am not an expert in C++ progamming myself, but I found the above works, if
you have a better solution, please let me know.

By now, if you make pytest again, it should work, and don't forget to 

{% highlight bash %}
$ make distribute
{% endhighlight %}

Finally, we're ready to try out one of the tutorials with our newly brewed
caffe.

{% highlight bash linenos %}
$ cd $CAFFE_ROOT
$ ./data/mnist/get_mnist.sh
$ ./examples/mnist/create_mnist.sh
$ ./examples/mnist/train_lenet.sh
{% endhighlight %}

While it's brewing, you could also use `nvidia-smi` to see how much the GPU is
being utilized.

At last, I have to admit that most of the tricks are more like hacks to the
linuxbrew formulas. Because I am not a ruby/linuxbrew expert, I tried to avoid
too much modification to the formula. Overall, I think linuxbrew + anaconda
really have saved me lots of time though there will still be glitches here and
there. I really appreciate the effort the developers who have put into those
projects. Imagine how much work it is going to be if you're gonna compile and
install all the stuffs and their dependencies from source mannually, a daunting
(also tedious) task.

I hope you find the above steps helpful. If you see any errors, please
let me know. Have fun brewing. :D
