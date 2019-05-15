---
layout: post
date: 2019-03-31 00:01
title: "激活函数的前世今生"
categories: ML
tag: 
	- Deep Learning
	- Activation Function
	- 激活函数
comment: true
---

本文主要介绍神经网络的激活函数。

<!--MORE -->

## 定义

在人工神经网络里面，每个神经元上都有一个Activation Function，并将Activation Function的值作为其输出。Activation Function的中文名叫激活函数，也不知是谁翻译过来的，叫的人多了，大家也就都懂了。激活函数分为线性激活函数（linear Activation Function）和非线性激活函数（Non-linear Activation Function），如果神经网络中使用的是linear Activation Function，那么每一层都相当于是上一层的线性组合，输入和输出均是线性的，此时就类似于Perceptron，那么hidden layer就没有什么意义，对于复杂的非线性问题则无法很好的解决。当我们使用非线性激活函数时，模型可以拟合任意函数的输出，表现空间更大，使用范围更广，效果更佳。下面根据激活函数的演进来看看各大激活函数的优缺点。

激活函数的类型特别多，本文主要介绍下面几个常用的：

1. sigmoid
2. tanh
3. ReLU (Rectified Linear Unit)
4. Leaky ReLU

## Sigmoid

sigmoid函数是一个有界可导的实函数，同时sigmoid函数还是单调的，其表达式如下：

$$
\sigma(x) = \frac{1}{1+e^{-x}}
\label{1}\tag{1}
$$

根据表达式，我们可以使用Python绘制出sigmoid曲线：

![](/assets/articleImg/2019/sigmoid-function.png)

从上图我们看出，sigmoid函数取值在0到1，因此我们一般使用sigmoid的值作为概率输出，如LR：

特点：

1. 导数$\sigma' = \sigma (1- \sigma)$
2. sigmoid单调，导数非单调；
3. 值域0到1之间，可表示概率；

绘制sigmoid曲线代码：

```
import matplotlib.pyplot as plt
import numpy as np

def sigmoid(x):
    return 1 / (1 + np.exp(-x))
x = np.arange(-5., 5., 0.1)
y_sig = sigmoid(x)
plt.plot(x, y_sig, 'b', label='sigmoid')
plt.grid()
plt.title("Sigmoid Activation Function")
plt.text(6, 0.8, r'$\sigma(x)=\frac{1}{1+e^{-x}}$', fontsize=15)
plt.legend(loc='lower right')
plt.show()
```

## tanh

$$
tanh(x) = \frac{1-e^{-2x}}{1+e^{-2x}}
\label{2}\tag{2}
$$

图型如下：

![](/assets/articleImg/2019/tanh-function.png)

1. 导数$tanh' = 1- tanh^2$
2. tanh单调函数，其导数非单调；
3. 中心为0；

绘制tanh曲线代码：

```
import matplotlib.pyplot as plt
import numpy as np

def tanh(x):
    return (1-np.exp(-2*x))/(1 + np.exp(-2*x))

x = np.arange(-5., 5., 0.1)
plt.grid()

plt.plot(x, tanh(x), 'g', label='tanh')
plt.title("tanh activation function")
plt.text(6,1, r'$tanh(x)=\frac{1-e^{-2x}}{1+e^{-2x}}$', fontsize=15)
plt.legend(loc='lower right')
plt.show()
```

## ReLU

$$
ReLU(x) = max(0, x)
\label{3}\tag{3}
$$

![](/assets/articleImg/2019/relu-function.png)

绘制ReLU曲线代码：


```
import matplotlib.pyplot as plt
import numpy as np

def relu(x):
    return np.maximum(0, x)

x = np.arange(-5., 5., 0.1)
y_relu = relu(x)
plt.plot(x, y_relu, 'r', label='ReLU')
plt.grid()
plt.title("ReLU Activation Function")
plt.text(6, 4, r'$relu(x)=max(0, x)$', fontsize=15)
plt.legend(loc='lower right')
plt.show()
```

## Leaky ReLU

$$
f(x) = max(\alpha x, x)
\label{4}\tag{4}
$$

![](/assets/articleImg/2019/leaky-relu-function.png)


绘制Leaky ReLU曲线代码：

```
import matplotlib.pyplot as plt
import numpy as np

def leaky_relu(x, a=0.1):
    return np.maximum(a*x, x)

x = np.arange(-5., 5., 0.1)
y_relu = leaky_relu(x)
plt.plot(x, y_relu, '#FFAA00', label='Leaky ReLU')
plt.grid()
plt.title("Leaky ReLU Activation Function")
plt.text(6, 4, r'$f(x)=max(ax, x)$', fontsize=15)
plt.legend(loc='lower right')
plt.show()
```
<!--more-->


## 扩展

https://en.wikipedia.org/wiki/Activation_function

![](/assets/articleImg/2019/activation-sheet.png)
Fig: Activation Function Cheetsheet

## References

1. [The equivalence of logistic regression and maximum entropy models](http://www.win-vector.com/dfiles/LogisticRegressionMaxEnt.pdf)
2. [The Origins of Logistic Regression - Tinbergen Institute](https://papers.tinbergen.nl/02119.pdf)