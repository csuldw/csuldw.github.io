---
layout: post
date: 2015-08-12 20:24
title: "从Theano到Lasagne：基于Python的深度学习的框架和库（译文）"
categories: ML
tag: 
	- Machine Learning
	- 框架&库
	- 译文
	- Theano
	- Lasagne
comment: true
---

英文链接：http://creative-punch.net/2015/07/frameworks-and-libraries-for-deep-learning/

深度学习是机器学习和人工智能的一种形式，利用堆积在彼此顶部的神经网络的多个隐藏层来尝试形成对数据更深层次的“理解”。

最近，深度神经网络以“Deep Dreams”形式在网站中如雨后春笋般出现，或是像谷歌研究原创论文中描述的那样：Inceptionism。

在这篇文章中，我们将讨论几个不同的深度学习框架，库以及工具。

<!--more-->

## Python深度学习

### Theano

主页：http://deeplearning.net/software/theano/

Github网址：https://github.com/Theano/Theano

Theano不仅是这篇文章中将要讨论的其他框架的核心库，于其自身而言，它也是一个强大的库，几乎能在任何情况下使用，从简单的logistic回归到建模并生成音乐和弦序列或是使用长短期记忆人工神经网络对电影收视率进行分类。

Theano大部分代码是使用Cython编写，Cython是一个可编译为本地可执行代码的Python方言，与仅仅使用解释性Python语言相比，它能够使运行速度快速提升。最重要的是，很多优化程序已经集成到Theano库中，它能够优化你的计算量并让你的运行时间保持最低。

如果速度的提升还不能满足你，它还内置支持使用CUDA在GPU上执行那些所有耗时的计算。所有的这一切仅仅只需要修改配置文件中的标志位即可。在CPU上运行一个脚本，然后切换到GPU，而对于你的代码，则不需要做任何变化。

同时我们应该注意到，尽管Theano使用Cython和CUDA对其性能大大提升，但你仍然可以仅仅使用Python语言来创建几乎任何类型的神经网络结构。

### Pylearn2

主页：http://deeplearning.net/software/pylearn2/

Github网址：https://github.com/lisa-lab/pylearn2

Pylearn2和Theano由同一个开发团队开发，Pylearn2是一个机器学习库，它把深度学习和人工智能研究许多常用的模型以及训练算法封装成一个单一的实验包，如随机梯度下降。

你也可以很轻松的围绕你的类和算法编写一个封装程序，为了能让它在Pylearn2上运行，你需要在一个单独的YAML格式的配置文件中配置你整个神经网络模型的参数。

除此之外，它还有很多数据集及其预编译好的软件包，所以，你现在就可以直接使用MNIST数据集开始做实验了！


### Blocks

Github网址：https://github.com/mila-udem/blocks

Blocks是一个非常模块化的框架，有助于你在Theano上建立神经网络。目前它支持并提供的功能有：

构建参数化Theano运算，称之为“bricks”。
在大型模型中使用模式匹配来选择变量以及“bricks”。
使用算法优化模型。
训练模型的保存和恢复。
在训练过程中检测和分析值（训练集以及测试集）。
图形变换的应用，如dropout。



### Keras

主页：http://keras.io/

Github网址：https://github.com/fchollet/keras

Keras是一个简约的、高度模块化的神经网络库，设计参考了Torch，基于Theano和Python语言编写，支持GPU和CPU。它的开发侧重于实现快速试验和创造新的深度学习模型。

如果你需要具有以下功能的深度学习库，采用Keras就恰到好处：

可以很容易地、快速地建立原型（通过总体模块化，极简化并且可扩展化）。
支持卷积网络和递归网络，以及两者的组合。
支持任意连接方式（包括多输入多输出训练）。
Keras库与其他采用Theano库的区别是Keras的编码风格非常简约、清晰。它把所有的要点使用小类封装起来，能够很容易地组合在一起并创造出一种全新的模型。


### Lasagne

Github网址：https://github.com/Lasagne/Lasagne

Lasagne不只是一个美味的意大利菜，也是一个与Blocks和Keras有着相似功能的深度学习库，但其在设计上与它们有些不同。

下面是Lasagne的一些设计目的：

简单化：它应该是易于使用和扩展的机器学习库。每添加一个特征，就应该考虑其对易用性和扩展性的影响。每一个抽象概念的加入都应该仔细检查，以确定增加的复杂性是否合理。
小接口：尽可能少的类和方法。尽可能依赖Theano的功能和数据类型，遵循Theano的规定。如果没有严格的必要，不要在类中封装东西。这会使它更容易使用库并且扩展它（不需要有太多的认知）。
不碍事：未使用的功能应该是不可见的，用户不会考虑他们不使用的功能。尽可能单独的使用库文件中的组件。
透明性：不要试图掩盖Theano，尽量以Python或NumPy数据类型的形式将函数和方法返回给Theano表达式。
重点：遵循Unix哲学“做一件事，并把它做好”，重点集中在前馈神经网络。
实用主义：使普通用例更易于使用，这要比支持每一个可能的用例更为重要。


译者简介： [刘帝伟](https://www.csuldw.com)，中南大学软件学院在读研究生，关注机器学习、数据挖掘及生物信息领域。

