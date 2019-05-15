---
layout: post
title: "Perceptron - 原理与实现"
date: 2018-01-29 00:22:22
categories: ML
tag:
  - Machine Learning
  - 感知机
  - 算法
comments: true
---

回顾了下以前的博文，发现自己CSDN博客里面有的博文没有同步到这里来。出于温故知新的目的，打算将perceptron引入至此，并在原来的基础上稍作更改，下面请看正文。

<!-- more -->


追根溯源，Perceptron是**Rosenblatt**于1957年提出，它是**神经网络**和**支持向量机**的基础，我们称之为”感知器“，或”感知机“。在机器学习理论中，**感知机**（perceptron）属于二分类方法，构造的模型属于线性分类模型，同时它也是监督学习算法的一种，其输入为样本实例的特征向量，输出为样本实例的类别label（取+1/-1或0/1）。感知机对应于在输入空间中将样本空间划分为两类的分离超平面，它旨在求出该超平面。为了求得此超平面，进一步引入了基于误分类的损失函数，并利用梯度下降法对损失函数进行最优化。感知机的学习算法具有简单而易于实现的优点。感知机预测则是用学习得到的感知机模型对新的样本实例进行预测的，属于判别式模型。

## **定义**

假设输入空间(特征向量)为$X\subseteq R^n$，输出空间为Y={-1,+1}。输入$x∈X$表示样本实例的特征向量，对应于输入空间的点；输出$y∈Y$表示样本的类别。输入空间到输出空间的决策函数为：

$$f(x)=sign(w·x + b)$$

这种模型称为感知机。其中，参数w叫做权值向量**weight**，b称为偏置**bias**。$w·x$表示w和x的**点积**

$$\sum_{i=1}^m w_i x_i= w_1x_1+w_2x_2+...+w_nx_n$$

**sign**为符号函数，公式如下：

$$
f(x)=
\begin{cases}
+1 & \text{if } z>0 \\\\
-1 & \text{else }
\end{cases}
$$


可以看到，一个感知机主要由如下三部分组成：

- 输入权值：$w$，叫做权值向量**(weight)**;
- 激活函数：感知器的激活函数可以有很多选择，比如这里我们选择是$sign$符号函数作为激活函数；
- 输出：$y=f(\mathrm{w}\cdot \mathrm{x}+b)\qquad $


在二分类问题中，$f(x)$的值（+1或-1）用于判别$x$为正样本（+1）还是负样本（-1）。刚刚说到，感知机是一种线性分类模型，属于判别模型。因此，我们需要做的就是找到一个最佳的满足$w \cdot x + b = 0$的权值向量w和偏置项b，也就是一条分离超平面（*separating hyperplane*）。如下图，一个线性可分的感知机模型

<center>
![这里写图片描述](/assets/articleImg/2018-01-29-1.png)
</center>

中间的直线即$w \cdot x + b = 0$这条直线。

线性分类器的几何表示有：直线、平面、超平面。

## **学习策略**

<font color="red">**核心：极小化损失函数。**</font>

如果训练集是可分的，感知机的学习目的是求得一个能将训练集正样本点和负样本点完全分开的分离平面（或超平面）。为了找到这样一个平面（或超平面），即确定感知机模型参数w和b，我们采用的是学习策略是极小化损失函数。

对于损失函数的选择，有多种，这里我们采用的是误分类点到超平面的距离，如下（可以自己推算一下，这里采用的是几何间距，就是点到直线的距离）：

$$\dfrac{1}{\parallel w\parallel}|w*x_{0}+b|$$

其中$||w||$是$L2$范数。

对于正确分类的样本点$(x_i, y_i)$而言：

$$y_i(w * x_i+b)>0$$

所以，对于误分类点$(x_i, y_i)$来说：

$$- y_i(w * x_i+b)>0 $$

误分类点$x_i$到超平面的距离为：

$$-\dfrac{1}{\parallel w\parallel}y_0(w*x_i+b)$$

那么，所有点到超平面的总距离为：

$$-\dfrac{1}{\parallel w\parallel} \sum_{x_i\epsilon M} y_i|w*x_i+b|$$

由于极值计算不受常量的影响，因此不考虑$\frac{1}{||w||}$,由此得到感知机的损失函数如下：

$$L(w,b) = - \sum_{x_i\epsilon M} y_i(w*x_i+b)$$

其中$M$为误分类的集合。这个损失函数就是感知机学习的**经验风险函数**。

可以看出，随时函数$L(w,b)$是非负的。<font color="red">**如果没有误分类点，则损失函数的值为0，而且误分类点越少，误分类点距离超平面就越近，损失函数值就越小**</font>。同时，损失函数$L(w, b)$是连续可导函数。

## **学习算法** 

通过上面的分析，得到了感知器的损失函数，由此将感知机学习转变成求解损失函数$L(w,b)$的最优化问题。通常采用的是SGD随机梯度下降法（stochastic gradient descent）。关于梯度下降的详细内容，参考[wikipedia Gradient descent](https://en.wikipedia.org/wiki/Gradient_descent)。下面给出一个简单的梯度下降的可视化图：

<center>
![这里写图片描述](/assets/articleImg/2018-01-29-2.png)
</center>

上图就是随机梯度下降法一步一步达到最优值的过程，说明一下，梯度下降其实是局部最优。感知机学习算法本身是误分类驱动的，因此我们采用随机梯度下降法。首先，任选一个超平面$w_0$和$b_0$，然后使用梯度下降法不断地**极小化目标函数**

$$min\_{w,b}  L(w,b) = - \sum\_{x\_i\epsilon M} y\_i(w*x\_0+b)$$

极小化过程不是一次使M中所有误分类点的梯度下降，而是一次随机的选取一个误分类点使其梯度下降。使用的规则为 $\theta: = \theta - \alpha \nabla\_\theta \ell (\theta)$，其中$\alpha$是步长，$\nabla_\theta \ell (\theta)$是梯度。假设误分类点集合$M$是固定的，那么损失函数$ L(w,b)$的梯度通过偏导计算：

$$\frac{\partial L(w,b)}{\partial w} = - \sum_{x_i\epsilon M}y_ix_i$$

$$\frac{\partial L(w,b)}{\partial b} = - \sum_{x_i\epsilon M}y_i$$

然后，随机选取一个误分类点，根据上面的规则，计算新的$w,b$，然后进行更新：

$$w :=  w + \eta y_i x_i$$
        	   
$$b :=  b + \eta y_i$$

其中$\eta$是步长，大于0小于1，在统计学习中称之为学习率（*learning rate*）。这样，通过迭代可以期待损失函数$L(w,b)$不断减小，直至逼近于0.

在有的文章中，最后得到的更新公式为：

$$w :=  w + \Delta w$$
               
$$\Delta w = \eta(t-o)x_i$$

<font color="red">**这是因为它采取的激活函数是正样本为1、负样本为0，所以权值的变化式子有点区别，不过思想都是一样的。**</font>

算法描述如下：

__算法：感知机学习算法原始形式__

```
输入：T={(x1,y1),(x2,y2)...(xN,yN)}（其中xi∈X=Rn，yi∈Y={-1, +1}，i=1,2...N，学习速率为η）
输出：w, b;感知机模型f(x)=sign(w·x+b)
(1) 初始化w0,b0，权值可以初始化为0或一个很小的随机数
(2) 在训练数据集中选取（x_i, y_i）
(3) 如果yi(w xi+b)≤0
           w = w + ηy_ix_i
           b = b + ηy_i
(4) 转至（2）,直至训练集中没有误分类点
```

解释：当一个实例点被误分类时，调整w,b，使分离超平面向该误分类点的一侧移动，以减少该误分类点与超平面的距离，直至超越该点被正确分类。

对于每个$w\cdot x$其实是这样子的（假设x表示的是n维）：

$$y\_j(t) = f[\mathbf{w}(t)\cdot\mathbf{x}\_j+b] = f[w\_1(t)x\_{j,1} + w\_2(t)x\_{j,2} + \dotsb + w\_n(t)x\_{j,n}+b]$$

对于输入的每个特征都附加一个权值，然后将相加得到一个和函数$f$，最后该函数的输出即为输出的$y$值。

## **实例**

正样本点：$x_1=(3,3)^T$,$x_2=(4,3)^T$
负样本点：$x_1=(1,1)^T$
求感知机模型$f(x)=sign(w\cdot x+b)$,其中$w=(w^{(1)},w^{(2)})^T,x=(x^{(1)},x^{(2)})^T$

**解答思路：**根据上面讲解的，写初始化权值w和偏置b，然后一步一步的更新权值，直到所有的点都分正确为止。

解：

（1） 令$w_0=0,b_0=0$
（2） 随机的取一个点，如$x_1$,计算$y_1(w_0\cdot x_1+b_0)$,结果为0，表示未被正确分类，根据下面的式子更新$w,b$<font color="red">（此例中，我们将学习率$\eta$设置为1）</font>:

$$w \leftarrow  w + \eta y_ix_i$$
       	   
$$b \leftarrow  b + \eta y_i$$

计算得到

$$w_1 = w_0 + \eta y_1x_1=(3,3)^T$$
       	   
$$b_1 =  b_0 + \eta y_1=1$$

得到一个模型

$$w\_1\cdot x+b\_1 = 3x\_{(1)}+3x\_{(2)}+1$$

（3）接着继续，计算各个点是否分错，通过计算得到，$x_1和x_2$两个点，$y_i(w_0\cdot x_i+b_1)$都大于0，所以是被正确分类的点，无需修改权值w和bias项；而对于$x_3$通过计算得到$y_3(w_0\cdot x_3+b_1)<0$,误分了，所以修改权值：

$$w_2 = w_1 + y_3x_3=(2,2)^T$$
       	   
$$b_2 =  b_1 + y_3=0$$

得到线性模型：

$$w\_2x+b\_2 = 2x\_{(1)}+2x\_{(2)}$$

一次下去，知道所有的点都有$y_i(w_0\cdot x_i+b_1)>0$即可，最后求得

$$w_7 = (1,1)^T,b_7=-3$$

所以感知机模型为：

$$f(x) = sign(x\_{(1)}+x\_{(2)}-3)$$

即我们所求的感知机模型。

## 源码实现

下面是感知器类的实现，详情请看注释。

```
# -*- coding: utf-8 -*-
"""
Created on Mon Jan 29 09:20:04 2018

@author: liudiwei
"""

class Perceptron(object):
    def __init__(self, input_num, activation_func):
        """init parameter
        activation_func: this is a function of activation
        input_num: number of sample
        """
        self.activation_func = activation_func
        self.weights = [0.0 for _ in range(input_num)]
        self.bias = 0.0           
    
    def train(self, input_vecs, labels, iteration, rate):
        """training model
        input_vec: input vetcor, a 2-D list
        labels: class label list
        iteration: 
        rate: learning rate
        """
        for i in range(iteration):
            self._one_iteration(input_vecs, labels, rate)
            
    def _one_iteration(self, input_vecs, labels, rate):
        """training model on input_vecs dataset"""
        samples = zip(input_vecs, labels)
        for (input_vec, class_label) in samples:
            output_val = self.predict(input_vec)
            self._update_weights(input_vec, output_val, class_label, rate)

    def _update_weights(self, input_vec, output_val, class_label, rate):
        """update weights for each iteration"""
        delta = class_label - output_val
        self.weights = map(lambda (x, w): w + rate * delta * x, 
                           zip(input_vec, self.weights))
        self.bias += rate * delta
    
    def __to_string__(self):
        return 'weights\t: %s\nbias\t: %f\n' % (self.weights, self.bias)
        
    def predict(self, input_vec):
        """input input_vec and return a prediction value"""
        return self.activation_func(
            reduce(lambda a, b: a + b,
                    map(lambda (x, w): x * w, zip(input_vec, self.weights)),
                    0.0) + self.bias)
```

## Conclusion

感知器Perceptron在机器学习当中是相当重要的基础，理解好感知器对后面的SVM和神经网络都有很大的帮助。事实上感知器学习就是一个损失函数的最优化问题，这里采用的是随机梯度下降法来优化。对于感知机的介绍，就到此为止，文章更新部分如有纰漏，还请指正！

## **References**

[1] 统计学习方法， 李航 著
[2] Wikiwand之Perceptron http://www.wikiwand.com/en/Perceptron
[3] Wikipedia https://en.wikipedia.org/wiki/Machine_learning