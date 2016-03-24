---
layout: post
date: 2016-03-05 03:04
title: "损失函数"
categories: ML
tag:
	- Machine Learning
	- Loss Function
comment: true
---


损失函数（loss function）是用来估量你模型的预测值f(x)与真实值Y的不一致程度，它是一个非负实值函数,通常使用L(Y,f(x))来表示。通常损失函数是经验风险表达式的一部分，而模型的结构风险函数都可以表示成如下式子：

![$$\theta^* = \arg \min_\theta \frac{1}{N}{}\sum_{i=1}^{N} L(y_i, f(x_i; \theta) + \lambda\  \Phi(\theta)$$](http://latex.codecogs.com/gif.latex?%24%24%5Ctheta%5E*%20%3D%20%5Carg%20%5Cmin_%5Ctheta%20%5Cfrac%7B1%7D%7BN%7D%7B%7D%5Csum_%7Bi%3D1%7D%5E%7BN%7D%20L%28y_i%2C%20f%28x_i%3B%20%5Ctheta%29%20&plus;%20%5Clambda%5C%20%5CPhi%28%5Ctheta%29%24%24)

其中，前面的均值函数表示的是经验风险函数，`L`代表的是损失函数，后面的$\Phi$是正则化项（regularizer）或者叫惩罚项（penalty term），它可以是`L1`，也可以是`L2`，或者是其他的正则函数。整个式子表示的是找到使目标函数最小时的$\theta$值。通常来说，对于不同的分类器，其损失函数各有不同，本文主要列出几种常见的损失函数。

<!-- more -->


### Logistic Regression逻辑回归

有些人可能觉得逻辑回归的损失函数就是平方损失，其实并不是这样子。平方损失是通过线性回归推导出来的，而逻辑回归得到的并不是平方损失。逻辑回归是假设样本服从伯努利分布，然后求得该分部下的似然函数，接着取对数，然而逻辑回归并没有极大化似然函数求极值，而是把极大化当做一种思想，进而得到它的经验风险函数就是最小化负的似然函数（即max F(y, f(x))  --->  min -F(y, f(x)))。换种角度，从损失函数的视角来看，就变成了log对数损失函数了。
  
<font color=green>**log对数损失函数的标准形式**</font>：

$$L(Y,P(Y|X)) = -\log P(Y|X)$$

如上所书，对数损失是用于最大似然估计的（因为对于MLE来说，一般都是先取对数再求导找极值点）。损失函数L(Y, P(Y|X))指的是X分类为Y的情况下，使P(Y|X)达到最大。而log是单调递增函数，所以logP(Y|X)也会达到最大值，因此前面加符号之后，最大化P(Y|X)就等同于最小化L。

logistic回归的P(Y=y|x)表达式如下：

$$P(Y=y|x) = \frac{1}{1+exp(-yf(x))}$$

将它带入到上式，通过推导可以得到logistic如下的损失函数表达式：

$$L(y,P(Y=y|x)) = \log (1+exp(-yf(x)))$$


总的来说，逻辑回归是通过对对数似然函数求导，最后得到的目标式子如下：

![$$J(\theta) = - \frac{1}{m}\left [ \sum_{i=1}^m y^{(i)} \log h_{\theta}(x^{(i)}) + (1-y^{(i)}) \log(1-h_{\theta}(x^{(i)}))  \right ]$$](http://latex.codecogs.com/gif.latex?J%28%5Ctheta%29%20%3D%20-%20%5Cfrac%7B1%7D%7Bm%7D%5Cleft%20%5B%20%5Csum_%7Bi%3D1%7D%5Em%20y%5E%7B%28i%29%7D%20%5Clog%20h_%7B%5Ctheta%7D%28x%5E%7B%28i%29%7D%29%20&plus;%20%281-y%5E%7B%28i%29%7D%29%20%5Clog%281-h_%7B%5Ctheta%7D%28x%5E%7B%28i%29%7D%29%29%20%5Cright%20%5D)

如果是二分类的话，则m取值为2，如果是多分类，m就相应的类别个数。这里 需要提及一下：之所以有人认为逻辑回归是平方损失，是因为当使用梯度下降来求最优解的时候，它的迭代式子与平方损失求导后的式子非常相似，从而给人一种错觉了。

- 参考 [Lecture 6: logistic regression](https://www.cs.berkeley.edu/~russell/classes/cs194/f11/lectures/CS194%20Fall%202011%20Lecture%2006.pdf)

### 最小二乘（Ordinary Least Squares, OLS）

最小二乘法是线性回归的一种，线性回归假设样本和噪声都服从高斯分布，最后通过极大似然估计（MLE）就可以推导出最小二乘。最小二乘的基本原则是：<font color="1986C7">**最优拟合直线应该是使各点到回归直线的距离和最小的直线，即平方和最小**</font>。也就是说OLS是基于距离的，这个距离就是我们用的最多的欧氏距离。至于为什么用欧式距离作为误差度量 （即Mean squared error， MSE），主要有以下几点：

- 简单，并且计算方便；
- 它是一种很好的相似性度量标准；
- 在不同的表示域变换后特征性质不变。

<font color=green>**平方损失（Square loss）的标准形式如下：**</font>

$$ L(Y, f(X)) = (Y - f(X))^2 $$

当样本数是n时，使用最小二乘时的损失函数为：

![$$L(Y, f(X)) = \sum _{i=1}^{n}(Y - f(X))^2$$](http://latex.codecogs.com/gif.latex?L%28Y%2C%20f%28X%29%29%20%3D%20%5Csum%20_%7Bi%3D1%7D%5E%7Bn%7D%28Y%20-%20f%28X%29%29%5E2)

Y-f(X)表示的是残差（GBDT）中常说的，整个式子表示的是<font color="#1986C7">**残差的平方和**</font>，而我们的目的就是让这个目标函数最小化（注：此处未加入正则化），实际上就是使残差平方和最小（residual sum of squares，RSS）。

有时会使用均方差（MSE），公式如下：

![](https://upload.wikimedia.org/math/0/6/8/0686d09b81bdb146174754ee2f74b81f.png)

注：我们通常说的线性有两种，一种是因变量y是自变量x的线性函数，一种是因变量y是参数$\alpha$的线性函数。在机器学习中，通常指的都是后一种。

### Boosting

<font color=green>**指数损失(exp-loss）**</font>

![$$L(y, f(x)) = \exp[-yf(x)]$$](http://latex.codecogs.com/gif.latex?L%28y%2C%20f%28x%29%29%20%3D%20%5Cexp%5B-yf%28x%29%5D)

Adaboost在给定n个样本的情况下，损失函数为：


![L(y, f(x)) = \frac{1}{n}\sum_{i=1}^{n}\exp[-y_if(x_i)]](http://latex.codecogs.com/gif.latex?L%28y%2C%20f%28x%29%29%20%3D%20%5Cfrac%7B1%7D%7Bn%7D%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%5Cexp%5B-y_if%28x_i%29%5D)


### SVM

<font color=green>**Hinge 损失函数**</font>


![$$L(y) = \max(0, 1-ty), t=\pm 1$$](http://latex.codecogs.com/gif.latex?L%28y%29%20%3D%20%5Cmax%280%2C%201-ty%29%2C%20t%3D%5Cpm%201)

可以看出，当|y|>=1时，L(y)=0。

几种误差函数的可视化图如下：

![$$E(z) = max(0, 1-z)$$](/assets/articleImg/hinge.png)

在图中是蓝色的线所表示的那个，the Log Loss 为红色的线所表示，而 the Square Loss 是绿色 the misclassification error 用黑色表示。更多关于Hinge loss function，请参考[Hinge-loss](https://en.wikipedia.org/wiki/Hinge_loss).


补充一下，在libsvm中一共有4中核函数可以选择，对应的是`-t`参数分别是：

- 0-线性核；
- 1-多项式核；
- 2-RBF核；
- 3-sigmoid核。

### 其它损失函数

除了以上这几种，常用的还有：

<font color=green>**0-1损失函数**</font>

![L(Y, f(X)) = \left\{\begin{matrix}1 ,& Y \neq f(X)\\ 0 ,& y = f(X)    \end{matrix}\right.](http://latex.codecogs.com/gif.latex?L%28Y%2C%20f%28X%29%29%20%3D%20%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D1%20%2C%26%20Y%20%5Cneq%20f%28X%29%5C%5C%200%20%2C%26%20y%20%3D%20f%28X%29%20%5Cend%7Bmatrix%7D%5Cright.)


<font color=green>**绝对值损失函数**</font>

![$$L(Y, f(X)) = |Y-f(X)|$$](http://latex.codecogs.com/gif.latex?L%28Y%2C%20f%28X%29%29%20%3D%20%7CY-f%28X%29%7C)


在实际应用中，要牢记，<font color="#007FFF">**参数越多，模型就越复杂，而越复杂的模型越容易过拟合**</font>.过拟合就是说模型在训练数据上的效果远远好于在测试集上的性能。此时使用正则化可以减小参数，简化模型，从而使模型具有更好的泛化能力。





## 参考文献

- https://github.com/JohnLangford/vowpal_wabbit/wiki/Loss-functions
- http://image.diku.dk/shark/sphinx_pages/build/html/rest_sources/tutorials/concepts/library_design/losses.html
- http://www.cs.cmu.edu/~yandongl/loss.html