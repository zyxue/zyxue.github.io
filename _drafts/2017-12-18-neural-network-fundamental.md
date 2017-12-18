---
layout: post
<!-- title:   -->
author: Zhuyi Xue
tags: linear algebra
---

I've implemented a basic neural network (NN) multiple times now along
with completing problem sets from different courses:

* [Andrew Ng's coursera course](https://www.coursera.org/learn/machine-learning), Machine Learning
* [CS231n](http://cs231n.stanford.edu/) Winter 2016, Convolutional
   Neural Networks for Visual Recognition
* [CS224n](http://web.stanford.edu/class/cs224n/) Winter 2017,
   Natural Language Processing with Deep Learning
* [CS229](http://cs229.stanford.edu/) Autumn 2017, Machine Learning

Here, I would like to summarize how to implement a basic
single-hidden-layer NN from scratch vanilla python and numpy. I think
it much more helpful for understanding the basics of NN than to use a
deep learning framework like
[tensorflow](https://www.tensorflow.org/), which usually works at a
higher level. Similar to my experience with web development, when
developing web application in a relatively lower-level web framework
(e.g. [webapp2](https://webapp2.readthedocs.io/en/latest/)), it
exposes more details of how different features work (e.g. user
authentification and authorization) than in a higher-level framework
(e.g. Django), which abstracts more details again. They always come
with pros and conds. Lower-level tools usually come with higher
flexibility, but higher-level tools come with higher
productivity. Choosing which tool to use highly depends on what we
want to do. Here, we want to absolutely master the basics of NN, so
it's better to start low.

You've probably seen a NN diagram before, like the
[one](http://neuralnetworksanddeeplearning.com/images/tikz16.png) from
[Chapter 2](http://neuralnetworksanddeeplearning.com/chap2.html) in
Michael Nielsen's online book of
[Neural Networks and Deep Learning](http://neuralnetworksanddeeplearning.com/index.html)
That kind of illustration is very common and it shows how a single
input sample is processed by a NN. However, in practice, people
usually process a batch of samples at a time, and I often feel there
is disconnet between such diagram of the implementation. So I think of
a different way of drawing NN, which I think maybe intuitive, like the
one below.

<img src="https://lh3.googleusercontent.com/WlNChYQgumrlpH4CD_1XbavDAW3GEgaVTaXEHTTBbwVEbNUv2y9zW4W8tkVmsIQXMdMR454v6Y1yzMFxtSPcT6keJV-xxYKQoBYFimE8pX6H9lSOMnfAbX3_0ua3FdsE30Qu7wWjUzZE8mrEB2dXLHPBhI6aP9vHRA3zQGFHYYOKTEEiUHa6DvXXlZh8e-0i0LIzgfX5AGejr-SaPyQRPhD5jDlv2xtHA2xHml50Ccu9gBInucyYkdUy_g-3oTB499Hs8csDAO7LwycBuC6QVHcz6fzaJQ62kylDYTM3PzsftSOmVAruf4DRMv1HlVSo4RgklONX9WSUxjleAt4TQ74JiC5UC4WUSOn2qiF4Ax_XnUYcsTbg7b9e5xyH0QxqjjFv1PzxQQnh-vhVlqWi-b7z6zXVpxKC_D5cxVHUliPwu71O04fsYajg9ogtQDdUa4u9KIsUujmxp_MK6i46Eg4IdnamwCn6xcCnL7E05IcKkTFLF36ge3Gekx-HMtutJjnmJ2WOl2SyWEZtOT_lAzvPW7hG-vKFdIZXmK3F4YaZVqn-YpJd4oNYA9_0YahvPlkbUB_BFVInsbgBbQshnJLTKyHt5CdW9yB97hUggg=w1280-h810-no" alt="alternative NN illustration">

Each row is a data point, by stacking them together, we have a batch
of samples. There should be connections in each row, but for clarity,
I only drew those in the first one.

Here, I also labeled the dimensions:

* $$B$$ is the batch size,
* $$D$$ is the dimension of input data, (e.g. for an 25x25 image, $$D = 725$$),
* $$H$$ is the hidden layer size, and
* $$K$$ is the dimension of output, e.g. for a binary classification
probalem, the $$K = 2$$.

The working of NN is basically like this, 

1. The first matrix, let's call it $$X$$ of shape $$B \times D$$, will
be multipled by the weight matrix $$W_1$$ and added by the bias
$$b_1$$, resulting in another matrix $$Z$$ of shape $$B \times H$$.
1. $$H$$ needs to be activated with an activation function
   (e.g. sigmoid) to obtain another matrix, $$Z$$. Note that
   activation is nonlinear and element-wise. If you focus on an single
   neural in the hidden layer, inspecting all its connection from the
   previous layer, the working of a single neuron can be summarized as
   a nonlinear transformation of linear weighted sum.
1. Then similar process is repeated between the hidden layer and
   output layer. However, since it's the output layer, there is no
   need to do activation anymore. Instead, we convert the linear
   weighted sum between 0 to 1 with a softmax function, which then can
   be interpreted as probability of belonging to a certain class.
   
To represent the above process with mathmatical symbols, I will
represent variables with small case letters and size with upper case
letters:

<script type="math/tex; mode=display">% <![CDATA[
\begin{align}
	\underset{B \times H}{z_1} &= \underset{B \times D}{x} \underset{D \times H}{w_1} + \underset{B \times H}{b_1} \\
	\underset{B \times H}{a_1} &= \mathrm{sigmoid}(\underset{B \times H}{z_1}) \\
	\underset{B \times K}{z_2} &= \underset{B \times H}{a_1} \underset{H \times K}{w_2} + \underset{B \times K}{b_2} \\
	\underset{B \times K}{\hat y} &= \mathrm{softmax}(\underset{B \times K}{z_2}) \\
\end{align}
%]]></script>	

Here, I also labeled the dimension of each matrix underneath
it. Subscript number means indicates the layer where the variable
belongs.

That is the forward propagation process. Now, let's do
backpropation. The math is a little bit more involved. Starting from
the last layer,

<script type="math/tex; mode=display">% <![CDATA[
\begin{align*}
\underset{B \times K}{\delta_1} &= \underset{B \times K}{\frac{\partial J}{\partial z_2}} = \underset{B \times K}{\hat y - y} \\
\underset{H \times K}{\frac{\partial J}{\partial W_2}} &= \delta_1 \frac{\partial z_2}{\partial W_2} = \underset{H \times B}{a_1^T} \underset{B \times K}{\delta_1} \\
\underset{B \times K}{\frac{\partial J}{\partial b_2}}  &= \delta_1 \frac{\partial z_2}{\partial b_2} = \underset{B \times K}{\delta_1} \\
\underset{B \times H}{\frac{\partial J}{\partial a_1}}  &= \delta_1 \frac{\partial z_2}{\partial a_1} = \underset{B \times K}{\delta_1} \underset{K \times H}{W_2^T} \\
\underset{B \times H}{\delta_2} &= \frac{\partial J}{\partial z_1}  = \frac{\partial J}{\partial h} \sigma'(z_1) =  \underset{B \times K}{\delta_1} \underset{K \times H}{W_2^T} \circ \underset{H \times H}{diag\{h(1 - h)\}} \\
\underset{D \times H}{\frac{\partial J}{\partial W_1}} &= \delta_2 \frac{\partial z_1}{\partial W_1} = \underset{D \times B}{x^T} \underset{B \times H}{\delta_2} \\
\underset{B \times H}{\frac{\partial J}{\partial b_1}} &= \delta_2 \frac{\partial z_1}{\partial b_1} = \underset{B \times H}{\delta_2} \\
\end{align*}
%]]></script>


Some 3rd-order tensor is involved. To be continued
