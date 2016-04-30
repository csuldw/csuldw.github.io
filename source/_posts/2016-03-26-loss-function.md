---
layout: post
title: "机器学习-损失函数"
date: 2016-03-26 13:04
categories: ML
tag:
	- Machine Learning
	- 损失函数
comment: true
---

损失函数（loss function）是用来估量你模型的预测值f(x)与真实值Y的不一致程度，它是一个非负实值函数,通常使用L(Y, f(x))来表示，损失函数越小，模型的鲁棒性就越好。损失函数是**经验风险函数**的核心部分，也是**结构风险函数**重要组成部分。模型的结构风险函数包括了经验风险项和正则项，通常可以表示成如下式子：
![$$\theta^* = \arg \min_\theta \frac{1}{N}{}\sum_{i=1}^{N} L(y_i, f(x_i; \theta) + \lambda\  \Phi(\theta)$$](http://latex.codecogs.com/gif.latex?%24%24%5Ctheta%5E*%20%3D%20%5Carg%20%5Cmin_%5Ctheta%20%5Cfrac%7B1%7D%7BN%7D%7B%7D%5Csum_%7Bi%3D1%7D%5E%7BN%7D%20L%28y_i%2C%20f%28x_i%3B%20%5Ctheta%29%20&plus;%20%5Clambda%5C%20%5CPhi%28%5Ctheta%29%24%24)

<!-- more -->

其中，前面的均值函数表示的是经验风险函数，L代表的是损失函数，后面的$\Phi$是正则化项（regularizer）或者叫惩罚项（penalty term），它可以是L1，也可以是L2，或者其他的正则函数。整个式子表示的意思是<font color="#1986C7">**找到使目标函数最小时的$\theta$值**</font>。下面主要列出几种常见的损失函数。


## 一、log对数损失函数（逻辑回归）

有些人可能觉得逻辑回归的损失函数就是平方损失，其实并不是。平方损失函数可以通过线性回归在假设样本是高斯分布的条件下推导得到，而逻辑回归得到的并不是平方损失。在逻辑回归的推导中，它假设样本服从<font color="#1986C7">**伯努利分布（0-1分布）**</font>，然后求得满足该分布的似然函数，接着取对数求极值等等。而逻辑回归并没有求似然函数的极值，而是把极大化当做是一种思想，进而推导出它的经验风险函数为：<font color="#1986C7">**最小化负的似然函数（即max F(y, f(x))  --->  min -F(y, f(x)))**</font>。从损失函数的视角来看，它就成了log损失函数了。

**log损失函数的标准形式**：
$$L(Y,P(Y|X)) = -\log P(Y|X)$$
刚刚说到，取对数是为了方便计算极大似然估计，因为在MLE中，直接求导比较困难，所以通常都是先取对数再求导找极值点。损失函数L(Y, P(Y|X))表达的是样本X在分类Y的情况下，使概率P(Y|X)达到最大值（换言之，<font color="#1986C7">**就是利用已知的样本分布，找到最有可能（即最大概率）导致这种分布的参数值；或者说什么样的参数才能使我们观测到目前这组数据的概率最大**</font>）。因为log函数是单调递增的，所以logP(Y|X)也会达到最大值，因此在前面加上负号之后，最大化P(Y|X)就等价于最小化L了。  
逻辑回归的P(Y=y|x)表达式如下：
$$P(Y=y|x) = \frac{1}{1+exp(-yf(x))}$$
将它带入到上式，通过推导可以得到logistic的损失函数表达式，如下：
$$L(y,P(Y=y|x)) = \log (1+exp(-yf(x)))$$
逻辑回归最后得到的目标式子如下：

![$$J(\theta) = - \frac{1}{m}\left [ \sum_{i=1}^m y^{(i)} \log h_{\theta}(x^{(i)}) + (1-y^{(i)}) \log(1-h_{\theta}(x^{(i)}))  \right ]$$](http://latex.codecogs.com/gif.latex?J%28%5Ctheta%29%20%3D%20-%20%5Cfrac%7B1%7D%7Bm%7D%5Cleft%20%5B%20%5Csum_%7Bi%3D1%7D%5Em%20y%5E%7B%28i%29%7D%20%5Clog%20h_%7B%5Ctheta%7D%28x%5E%7B%28i%29%7D%29%20&plus;%20%281-y%5E%7B%28i%29%7D%29%20%5Clog%281-h_%7B%5Ctheta%7D%28x%5E%7B%28i%29%7D%29%29%20%5Cright%20%5D)

如果是二分类的话，则m值等于2，如果是多分类，m就是相应的类别总个数。这里需要解释一下：<font color="green">**之所以有人认为逻辑回归是平方损失，是因为在使用梯度下降来求最优解的时候，它的迭代式子与平方损失求导后的式子非常相似，从而给人一种直观上的错觉**</font>。

这里有个PDF可以参考一下：[Lecture 6: logistic regression.pdf](https://www.cs.berkeley.edu/~russell/classes/cs194/f11/lectures/CS194%20Fall%202011%20Lecture%2006.pdf).

## 二、平方损失函数（最小二乘法, Ordinary Least Squares ）

最小二乘法是线性回归的一种，OLS将问题转化成了一个凸优化问题。在线性回归中，它假设样本和噪声都服从高斯分布（为什么假设成高斯分布呢？其实这里隐藏了一个小知识点，就是**中心极限定理**，可以参考[【central limit theorem】](https://en.wikipedia.org/wiki/Central_limit_theorem)），最后通过极大似然估计（MLE）可以推导出最小二乘式子。最小二乘的基本原则是：<font color="1986C7">**最优拟合直线应该是使各点到回归直线的距离和最小的直线，即平方和最小**</font>。换言之，OLS是基于距离的，而这个距离就是我们用的最多的欧几里得距离。为什么它会选择使用欧式距离作为误差度量呢（即Mean squared error， MSE），主要有以下几个原因：

- 简单，计算方便；
- 欧氏距离是一种很好的相似性度量标准；
- 在不同的表示域变换后特征性质不变。

**平方损失（Square loss）的标准形式如下：**  
$$ L(Y, f(X)) = (Y - f(X))^2 $$
当样本个数为n时，此时的损失函数变为：  
![$$L(Y, f(X)) = \sum _{i=1}^{n}(Y - f(X))^2$$](http://latex.codecogs.com/gif.latex?L%28Y%2C%20f%28X%29%29%20%3D%20%5Csum%20_%7Bi%3D1%7D%5E%7Bn%7D%28Y%20-%20f%28X%29%29%5E2)    
`Y-f(X)`表示的是残差，整个式子表示的是<font color="#1986C7">**残差的平方和**</font>，而我们的目的就是最小化这个目标函数值（注：该式子未加入正则项），也就是最小化残差的平方和（residual sum of squares，RSS）。

而在实际应用中，通常会使用均方差（MSE）作为一项衡量指标，公式如下：  
$$MSE = \frac{1}{n} \sum_{i=1} ^{n} (\tilde{Y_i} - Y_i )^2$$  
上面提到了线性回归，这里额外补充一句，我们通常说的线性有两种情况，一种是因变量y是自变量x的线性函数，一种是因变量y是参数$\alpha$的线性函数。在机器学习中，通常指的都是后一种情况。



## 三、指数损失函数（Adaboost）

学过Adaboost算法的人都知道，它是前向分步加法算法的特例，是一个加和模型，损失函数就是指数函数。在Adaboost中，经过m此迭代之后，可以得到$f_{m} (x)$:

![$$f_m (x) = f_{m-1}(x) + \alpha_m G_m(x)$$](http://latex.codecogs.com/gif.latex?%24%24f_m%20%28x%29%20%3D%20f_%7Bm-1%7D%28x%29%20&plus;%20%5Calpha_m%20G_m%28x%29%24%24)

Adaboost每次迭代时的目的是为了找到最小化下列式子时的参数$\alpha$ 和G：

![$$\arg \min_{\alpha, G} = \sum_{i=1}^{N} exp[-y_{i} (f_{m-1}(x_i) + \alpha G(x_{i}))]$$](http://latex.codecogs.com/gif.latex?%24%24%5Carg%20%5Cmin_%7B%5Calpha%2C%20G%7D%20%3D%20%5Csum_%7Bi%3D1%7D%5E%7BN%7D%20exp%5B-y_%7Bi%7D%20%28f_%7Bm-1%7D%28x_i%29%20&plus;%20%5Calpha%20G%28x_%7Bi%7D%29%29%5D%24%24)

**而指数损失函数(exp-loss）的标准形式如下**

![$$L(y, f(x)) = \exp[-yf(x)]$$](http://latex.codecogs.com/gif.latex?L%28y%2C%20f%28x%29%29%20%3D%20%5Cexp%5B-yf%28x%29%5D)

可以看出，Adaboost的目标式子就是指数损失，在给定n个样本的情况下，Adaboost的损失函数为：

![L(y, f(x)) = \frac{1}{n}\sum_{i=1}^{n}\exp[-y_if(x_i)]](http://latex.codecogs.com/gif.latex?L%28y%2C%20f%28x%29%29%20%3D%20%5Cfrac%7B1%7D%7Bn%7D%5Csum_%7Bi%3D1%7D%5E%7Bn%7D%5Cexp%5B-y_if%28x_i%29%5D)

关于Adaboost的推导，可以参考Wikipedia：[AdaBoost](https://en.wikipedia.org/wiki/AdaBoost)或者《统计学习方法》P145.

## 四、Hinge损失函数（SVM）


在机器学习算法中，hinge损失函数和SVM是息息相关的。在**线性支持向量机**中，最优化问题可以等价于下列式子：  
![$$\min_{w,b}  \ \sum_{i}^{N} [1 - y_i(w\cdot x_i + b)]_{+} + \lambda||w||^2 $$](http://latex.codecogs.com/gif.latex?%24%24%5Cmin_%7Bw%2Cb%7D%20%5C%20%5Csum_%7Bi%7D%5E%7BN%7D%20%5B1%20-%20y_i%28w%5Ccdot%20x_i%20&plus;%20b%29%5D_%7B&plus;%7D%20&plus;%20%5Clambda%7C%7Cw%7C%7C%5E2%20%24%24)  
下面来对式子做个变形，令：  
![$$[1 - y_i(w\cdot x_i + b)]_{+} = \xi_{i}$$](http://latex.codecogs.com/gif.latex?%24%24%5B1%20-%20y_i%28w%5Ccdot%20x_i%20&plus;%20b%29%5D_%7B&plus;%7D%20%3D%20%5Cxi_%7Bi%7D%24%24)  
于是，原式就变成了：  
![$$\min_{w,b}  \ \sum_{i}^{N} \xi_i + \lambda||w||^2 $$](http://latex.codecogs.com/gif.latex?%24%24%5Cmin_%7Bw%2Cb%7D%20%5C%20%5Csum_%7Bi%7D%5E%7BN%7D%20%5Cxi_i%20&plus;%20%5Clambda%7C%7Cw%7C%7C%5E2%20%24%24)  
如若取$\lambda=\frac{1}{2C}$，式子就可以表示成：  
![$$\min_{w,b}  \frac{1}{C}\left ( \frac{1}{2}\ ||w||^2 $$ + C \sum_{i}^{N} \xi_i\right )$$](http://latex.codecogs.com/gif.latex?%24%24%5Cmin_%7Bw%2Cb%7D%20%5Cfrac%7B1%7D%7BC%7D%5Cleft%20%28%20%5Cfrac%7B1%7D%7B2%7D%5C%20%7C%7Cw%7C%7C%5E2%20%24%24%20&plus;%20C%20%5Csum_%7Bi%7D%5E%7BN%7D%20%5Cxi_i%5Cright%20%29%24%24)  
可以看出，该式子与下式非常相似：  
![$$\frac{1}{m} \sum_{i=1}^{m} l(w \cdot  x_i + b, y_i) + ||w||^2$$](http://latex.codecogs.com/gif.latex?%5Cfrac%7B1%7D%7Bm%7D%20%5Csum_%7Bi%3D1%7D%5E%7Bm%7D%20l%28w%20%5Ccdot%20x_i%20&plus;%20b%2C%20y_i%29%20&plus;%20%7C%7Cw%7C%7C%5E2)

前半部分中的$l$就是hinge损失函数，而后面相当于L2正则项。

**Hinge 损失函数的标准形式**  
$$L(y) = \max(0, 1-y\tilde{y}), y=\pm 1$$  
可以看出，当|y|>=1时，L(y)=0。

更多内容，参考[Hinge-loss](https://en.wikipedia.org/wiki/Hinge_loss)。

补充一下：在libsvm中一共有4中核函数可以选择，对应的是`-t`参数分别是：

- 0-线性核；
- 1-多项式核；
- 2-RBF核；
- 3-sigmoid核。



## 五、其它损失函数

除了以上这几种损失函数，常用的还有：

**0-1损失函数**  
![L(Y, f(X)) = \left\{\begin{matrix}1 ,& Y \neq f(X)\\ 0 ,& y = f(X)    \end{matrix}\right.](http://latex.codecogs.com/gif.latex?L%28Y%2C%20f%28X%29%29%20%3D%20%5Cleft%5C%7B%5Cbegin%7Bmatrix%7D1%20%2C%26%20Y%20%5Cneq%20f%28X%29%5C%5C%200%20%2C%26%20y%20%3D%20f%28X%29%20%5Cend%7Bmatrix%7D%5Cright.)  
**绝对值损失函数**  
![$$L(Y, f(X)) = |Y-f(X)|$$](http://latex.codecogs.com/gif.latex?L%28Y%2C%20f%28X%29%29%20%3D%20%7CY-f%28X%29%7C)  
下面来看看几种损失函数的可视化图像，对着图看看横坐标，看看纵坐标，再看看每条线都表示什么损失函数，多看几次好好消化消化。  
![](/assets/articleImg/4DFDU.png)  
OK，暂时先写到这里，休息下。最后，需要记住的是：<font color="#1986C7">**参数越多，模型越复杂，而越复杂的模型越容易过拟合**</font>。过拟合就是说模型在训练数据上的效果远远好于在测试集上的性能。此时可以考虑正则化，通过设置正则项前面的hyper parameter，来权衡损失函数和正则项，减小参数规模，达到模型简化的目的，从而使模型具有更好的泛化能力。


## 参考文献

- https://github.com/JohnLangford/vowpal_wabbit/wiki/Loss-functions
- [library_design/losses](http://image.diku.dk/shark/sphinx_pages/build/html/rest_sources/tutorials/concepts/library_design/losses.html)
- http://www.cs.cmu.edu/~yandongl/loss.html
- http://math.stackexchange.com/questions/782586/how-do-you-minimize-hinge-loss
- 《统计学习方法》 李航 著.

---