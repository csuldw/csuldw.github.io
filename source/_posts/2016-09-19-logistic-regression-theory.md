---
layout: post
date: 2016-09-19 18:24
title: "Logistic Regression Theory"
categories: ML
tag: 
	- 逻辑回归
	- Logistic Regression
	- Gradient descent
	- Sigmoid
comment: true
---

出于学习的目的，笔者决定将逻辑回归总结一次。本文主要介绍逻辑回归的推导，囊括Sigmoid函数、极大似然估计、损失函数以、梯度下降以及正则化。文章内容纯属总结性知识，并不是对LR进行大篇长论。如有理解不到位的地方，还请读者指出。

<!-- more -->

什么是逻辑回归？引用[StatisticsSolutions](http://www.statisticssolutions.com/what-is-logistic-regression/)的解释就是：

> Logistic regression is the appropriate regression analysis to conduct when the dependent variable is dichotomous (binary).  Like all regression analyses, the logistic regression is a predictive analysis.  Logistic regression is used to describe data and to explain the relationship between one dependent binary variable and one or more metric (interval or ratio scale) independent variables.

通常监督学习问题可归纳为以下三个步骤：

- 寻找假设空间中的$h$函数（即hypothesis）；
- 根据已知条件构造损失函数$J(\theta)$；
- 最小化损失函数，即求解使得$J(\theta)$最小时的回归参数$\theta$.

## Sigmoid 函数

说到逻辑回归，Sigmoid是一大要点，其表达式为：

$$ g(z) = \frac{1}{1 + e^{-z} }
\tag{1} \label{1}$$

它是一个可导函数，定义域为$(-\infty, +\infty)$，值域为[0, 1]，其导数为：

$$g'(z) = g(z)(1-g(z))
\tag{2} \label{2}$$

说明一下，表达式$\eqref{1}$等价于使用线性回归模型的预测结果直接去逼近真实标记的对数几率，因此将其称作“对数几率回归（logit regression）”。使用这种方法有以下几大优点：

- 直接对样本进行建模，无需对样本进行先验假设；
- 其结果不仅可以预测出“label”，还可以得到近似的概率预测值；
- sigmoid函数的数学性质良好，它是任意阶可导的凸函数，因此许多的优化方法都可使用。

## 极大似然估计MLE与损失函数

在机器学习理论中，损失函数（loss function）是用来估量你模型的预测值$f(x)$与真实值$Y$的不一致程度，它是一个非负实值函数，通常使用$L(Y, f(x))$来表示，损失函数越小，模型的鲁棒性就越好。损失函数是**经验风险函数**的核心部分，也是**结构风险函数**重要组成部分。模型的结构风险函数包括了经验风险项和正则项，通常可以表示成如下式子：

$$\theta^* = \arg \min\_\theta \frac{1}{N}{}\sum_{i=1}^{N} L(y_i, f(x_i; \theta)) + \lambda\  \Phi(\theta)
\tag{3}\label{3}$$

对于逻辑回归，其loss function是log损失，可以通过极大似然估计进行推导得到。首先，给定一个样本$x$，可以使用一个线性函数对自变量进行线性组合，

$$\theta\_0 + \theta\_1 x\_1 + \theta\_2 x\_2 + \cdots + \theta\_nx\_n  = \theta^T x  
\tag{4} \label{4}$$

根据sigmoid函数，我们可以得出预测函数的表达式：

$$h\_{\theta}(x) = g(\theta^Tx) = \frac{1}{1 + e^{-\theta^Tx}}
\tag{5} \label{5}$$

式$\eqref{4}$表示$y=1$时预测函数为$h\_{\theta}(x)$。在这里，假设因变量$y$服从伯努利分布，取值为$0$和$1$，那么可以得到下列两个式子：

$$p(y=1 | x) = h\_{\theta} (x)
\tag{6} \label{6}$$

$$p(y=0 | x) = 1 - h\_{\theta} (x) 
\tag{7} \label{7}$$

而对于上面的两个表达式，通过观察，我们发现，可以将其合并为表达式$\eqref{7}$：

$$p(y | x) = h\_{\theta} (x)^y (1-h\_{\theta} (x))^{1-y}
\tag{8} \label{8}$$

根据上面的式子，给定一定的样本之后，我们可以构造出似然函数，然后可以使用极大似然估计MLE的思想来求解参数。但是，为了满足最小化风险理论，我们可以将MLE的思想转化为最小化风险化理论，最大化似然函数其实就等价于最小化负的似然函数。对于MLE，**就是利用已知的样本分布，找到最有可能（即最大概率）导致这种分布的参数值；或者说是什么样的参数才能使我们观测到目前这组数据的概率最大**。使用MLE推导LR的loss function的过程如下。

首先，根据上面的假设，写出相应的极大似然函数（假定有$m$个样本）：

$$
\begin{aligned} 
L(\theta) 
&=  \prod\_{i=1}^{m} p(y^{(i)} | x^{(i)}; \theta)  \\\\
&=  \prod\_{i=1}^{m} h\_{\theta} (x^{(i)})^{y^{(i)}} (1-h\_{\theta} (x^{(i)}))^{1-y^{(i)}} \\\\
\end{aligned}
\tag{9} \label{9}$$

直接对上面的式子求导会不方便，因此，为了便于计算，我们可以对似然函数取对数，经过化简可以得到下式的推导结果：

$$
\begin{aligned} 
\log L(\theta) 
&= \sum\_{i=1}^{m} \log \left [ (h\_{\theta} (x\_i)^{y^{(i)}} (1-h\_{\theta} (x^{(i)}))^{1-y^{(i)}}) \right ] \\\\
&= \sum\_{i=1}^{m} \left [ y^{(i)} \log h\_{\theta} (x^{(i)}) +  (1-y^{(i)}) \log(1-h\_{\theta} (x^{(i)})) \right ]  \\\\
\end{aligned}
\tag{10} \label{10}$$

因此，损失函数可以通过最小化负的似然函数得到，即下式：

$$J(\theta) = - \frac{1}{m} \sum\_{i=1}^m \left [ y^{(i)} \log h\_{\theta}(x^{(i)}) + (1-y^{(i)}) \log(1-h\_{\theta}(x^{(i)}))  \right ] 
\tag{11} \label{11}$$

在有的资料上，还有另一种损失函数的表达形式，但本质是一样的，如下：


$$J(\theta) = \frac{1}{m} \sum\_{i=1}^m log(1 + e^{-y^{(i)} \theta^T x})$$

对于初学者而言，可能会认为逻辑回归的随时函数是平方损失，这里解释一下：**之所以有人认为逻辑回归是平方损失，是因为在使用梯度下降来求最优解的时候，它的迭代式子与平方损失求导后的式子非常相似，从而给人一种直观上的错觉**。更多关于损失函数的内容，请移步至博主的另一篇博文[机器学习-损失函数](http://www.csuldw.com/2016/03/26/2016-03-26-loss-function/)。

## Gradient descent

梯度下降法又叫做最速下降法，为了求解使损失函数$J(\theta)$最小时的参数$\theta$，这里就以**梯度下降**为例进行求解，其迭代公式的推导过程如下：

$$
\begin{aligned} 
\frac{ \partial J(\theta)} {\partial \theta\_j} 
&= -\frac{1}{m} \sum\_{i}^{m} \left [ y^{(i)}(1 - h\_{\theta}(x^{(i)})) \cdot (-x\_{j}^{(i)}) + (1 - y^{(i)}) h\_{\theta} (x^{(i)})  \cdot (x\_{j}^{(i)}) \right ]  \\\\
&= - \frac{1}{m} \sum\_{i}^{m}  (-y^{(i)} \cdot x\_j^{(i)} + h\_{\theta}(x^{(i)}) \cdot x\_j^{(i)})  \\\\
&= -\frac{1}{m} \sum\_{i}^{m} (h\_{\theta}(x^{(i)}) - y^{(i)}) x\_j^{(i)} 
\end{aligned}
\tag{12} \label{12}$$


通过上面得到，可以得到最后的迭代式子：

$$\theta\_j = \theta\_j - \alpha \sum\_{i=1}^{m} (h\_{\theta}(x^{(i)}） - y^{(i)}) x\_j^{(i)} 
\tag{13}\label{13}$$

其中$\alpha$是步长。

最优化算法并不限于梯度下降，还有：

- Newton Method（牛顿法）
- Conjugate gradient method(共轭梯度法)
- Quasi-Newton Method(拟牛顿法)
- BFGS Method
- L-BFGS(Limited-memory BFGS)

上述优化算法中，BFGS与L-BFGS均由拟牛顿法引申出来，与梯度下降算法相比，其优点是：第一、不需要手动的选择步长；第二、比梯度下降算法快。但缺点是这些算法更加复杂，实用性不如梯度下降。

## Regulization

上面提到了，结构风险函数包括了经验风险项和正则项，加入正则项相当于对参数加入了一个先验分布，常用的有L1和L2正则，L1，L2正则化项对模型的参数向量进行“惩罚”，从而避免单纯最小二乘问题的过拟合问题。正则化项本质上是一种先验信息，整个最优化问题从贝叶斯观点来看是一种**贝叶斯最大后验估计**，其中正则化项对应后验估计中的先验信息，损失函数对应后验估计中的似然函数，两者的乘积即对应贝叶斯最大后验估计的形式，如果将这个贝叶斯最大后验估计的形式取对数，即进行极大似然估计，你就会发现问题立马变成了损失函数+正则化项的最优化问题形式。

在逻辑回归求解中，如果对样本加上一个先验的服从高斯分布的假设，那么就取log后，式子经过化简后就变成在经验风险项后面加上一个正则项，此时损失函数变为。

$$J(\theta) = - \frac{1}{m}\sum\_{i=1}^m  \left [ y^{(i)} \log h\_{\theta}(x^{(i)}) + (1-y^{(i)}) \log(1-h\_{\theta}(x^{(i)}))  \right ] + \frac{\lambda}{2m} \sum\_{j=1}^{n} \theta\_{j}^2
\tag{14}\label{14}$$

关于LR，这里还有个PDF可以参考一下：[Lecture 6: logistic regression.pdf](https://www.cs.berkeley.edu/~russell/classes/cs194/f11/lectures/CS194%20Fall%202011%20Lecture%2006.pdf)。

## References

- Andrew Ng《机器学习》笔记
- [https://en.wikipedia.org/wiki/Logistic_regression](https://en.wikipedia.org/wiki/Logistic_regression)
- [https://github.com/JohnLangford/vowpal_wabbit/wiki/Loss-functions](https://github.com/JohnLangford/vowpal_wabbit/wiki/Loss-functions)
- [Lecture 6: logistic regression.pdf](https://www.cs.berkeley.edu/~russell/classes/cs194/f11/lectures/CS194%20Fall%202011%20Lecture%2006.pdf)




