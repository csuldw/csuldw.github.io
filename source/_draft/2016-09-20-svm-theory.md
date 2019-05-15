---
layout: post
date: 2016-09-20 08:10
title: "SVM理论学习"
categories: ML
tag:
	- Machine Learning
	- SVM
	- 支持向量机
	- 最大间隔
	- 对偶
comment: true
---


SVM，全称是" [Support Vector Machine](http://en.wikipedia.org/wiki/Support_vector_machine)"，译作“支持向量机”。记得初次接触它的时候，还是两年前，导师要求每个人做一次报告，而我却不知不觉地选择了SVM。一直以来，对于SVM可以说是“又爱又恨”，“爱她”是因为她的能力确实惊人的强大，在很多场景下，都能做出非常不错的成果；“恨她”则是因为她背后的理论太过深奥，涉及的知识点比比皆是，让人难以琢磨。记得以前也写过一篇关于SVM的文章，只是那时候很多东西都是旁征博引，并且自己对SVM的理解也并不深刻，导致文章质量显得有些低劣。因此，为了加深对SVM的理解，同时提高文章的质量，笔者决定再次整理笔记，以便后续学习。

<!-- more -->

说到SVM，确实不容小觑，理论上涉及的知识点确实很多，主要包括间隔最大化、支持向量机、凸优化、拉格朗日乘子法、对偶、软间隔、核函数、SMO，并且每个点都可以单独成文。不过，笔者为了便于后续回顾SVM，不打算将它的内容单独成文，而是分成以下几个小结逐个进行阐述：

1. [最大间隔与支持向量](#最大间隔与支持向量)
2. [线性SVM与对偶](#线性SVM与对偶)
3. [软间隔与损失函数](#软间隔与损失函数)
4. [非线性SVM与核函数](#非线性SVM与核函数)
5. [SMO算法](#SMO算法)
6. [总结](#总结)

考虑到推导上面的复杂性，本文决定以简单的二分类为例。下面先给定一些数据假设。

> 假定训练数据$D=\lbrace (x\_1, y\_1), (x\_2, y\_2), \cdots, (x\_n, y\_n) \rbrace$, 其中$x\_i = \lbrace x\_{i1}, x\_{i2}, \cdots, x\_{id} \rbrace \in \chi = \mathbb{R} $，$d$为特征总数，$y\_i \in \lbrace -1, +1 \rbrace$, $1 \leq i \leq n$，我们把$x\_i$称为样本实例，$y\_i$称为类别标记，当$y\_i = +1$时，称$x\_i$为正样本；当$y\_i = -1$时，称$x\_i$为负样本，$(x\_i, y\_i)$则为一个样本点。 

在下面的篇幅中，文章会从数据是否满足线性可分进行讨论，当数据线性可分时，引入[最大间隔与支持向量](#最大间隔与支持向量)和[线性SVM与对偶](#线性SVM与对偶)两部分；；当数据线性不可分时，再引入[软间隔理论](#软间隔与损失函数)和[核函数](#非线性SVM与核函数)的思想。


## 最大间隔与支持向量

### 几何间隔与函数间隔

我们知道，当数据线性可分时，我们可以使用超平面$\mathbb{L}: w^{T}x + b = 0$来划分原超平面$\mathbb{S}$，其中$w = (w\_1, w\_2, \cdots, w\_d)^T$，该变量决定超平面$\mathbb{L}$的方向；$b$为偏置，描述的是超平面$\mathbb{L}$距离原点$O$的距离，而这个距离就是所谓的几何距离。在很多参考书籍中，都是从函数间隔逐步推导出几何间隔。实际上，几何间隔我们很早就接触了，也就是以前学几何学时点到直线的距离，如下式：

$$ d= \frac{|w^T x+b|}{||w||_2} \tag{1} \label{1}$$

式$\eqref{1}$中，分子是绝对值，分母就是$L2$范数。由于是二分类，$y\_i$为$\pm 1$，在使用超平面$\mathbb{L}$划分远超平面$\mathbb{S}$时，如果数据被分为正样本，则$w^{T}x + b \geq 1$，如果数据被分为负样本，则$w^{T}x + b \leq -1$，即：

$$
\begin{cases}
w^T x + b \geq +1, & y\_i = +1 \\\\
w^T x + b \leq -1, & y\_i = -1 \\\\
\end{cases}
\tag{2}\label{2}$$

可以看到，对于任意的样本$x\_i$，只要用$y\_i$乘以$\eqref{1}$式中的分子便可将绝对值符号消掉，于是就得到了下面的几何间隔表达式：

$$ \gamma\_i = y\_{i} \left ( \frac{w^T}{||w||\_2} \cdot  x\_i + \frac{b}{||w||\_2}\right ) \tag{3} \label{3}$$

下面我们的目的就是最小化这个几何间隔。在公式$\eqref{3}$中，已经携带了函数间隔，几何间隔仅仅是在函数间隔的基础上除了个$L2$范数$||w||$。函数间隔如下：

$$\widetilde{\gamma\_i} = y\_{i} \left ( w^T  x\_i + b\right )  \tag{4} \label{4}$$

所以几何间隔可以用函数间隔表示：

$$\gamma = \frac{\widetilde{\gamma}}{||w||}\tag{5} \label{5}$$

SVM的基本思想就是：求解能够正确划分训练数据集并且几何间隔最大的分离超平面$\mathbb{L}$。下面，我们需要做的就是最大化该几何间隔$\gamma$。

### 间隔最大化

根据上面的推导，我们的目的是最大化几何间隔，不过推导时还有一些附加条件，也就是我们希望分离超平面$\mathbb{L}$与每个训练样本的几何距离至少是$\gamma$。这样我们最大化几何间隔之后，才能更加明确地分离正负样本。将其转化为数学符号之后，得到下列最优化表达式：

$$
\begin{aligned} 
&\underset{w,b}{max} \ \ \gamma \\\\
&s.t. \ \ \ y\_{i} \left ( \frac{w^T}{||w||} \cdot  x\_i + \frac{b}{||w||}\right ) \geq \gamma \ \ \ ,i=1,2,\cdots,n
\end{aligned}
\tag{6} \label{6}
$$

将$\eqref{5}$式代入$\eqref{6}$式，便可得到问题的另一种表达形式：

$$
\begin{aligned}
&\underset{w,b}{max}  \ \ \dfrac{\widetilde{\gamma}}{||w||} \\\\
&s.t. \ \ \ y\_i(w^T x\_i+b)\geq \widetilde{\gamma} ,i=1,2, \cdots,n
\end{aligned}
\tag{7} \label{7}
$$

对于公式$\eqref{7}$，当函数间隔中的$w$和$b$按一定比例增长时，函数间隔也会随之同比增加，因此为了方便起见，可取函数间隔$\widetilde{\gamma}=1$，其结果也不会影响目标函数的最优化问题，从而得到下列式子：

$$  
\begin{aligned}
&\underset{w,b}{max} \ \ \dfrac{1}{||w||} \\\\ 
&s.t. \ \ \ y\_i(w^T x\_i+b)\geq 1 ,i=1,2,\cdots,n
\end{aligned}
\tag{8}\label{8}
$$

接着来分析公式$\eqref{8}$，因为最大化$\frac{1}{||w||}$其实和最小化$\frac{1}{2} ||w||^2$ 是等价的，所以可以将最大化问题转为最小化问题（目的是将其变成一个凸二次优化问题，方便求解，因为凸二次规划问题比较好解。凸二次规划指的是，最小化的那个函数是凸函数，同时它的不等式约束也是凸函数，它的等式约束则是$\mathcal{R}^n$ 上的仿射函数。可参考这份PPT [optimization - ppt](https://www.princeton.edu/~chiangm/optimization.pdf)）：

$$ 
\begin{aligned}
&\underset{w,b}{min} \ \ \frac{1}{2} ||w||^{2} \\\\ 
&s.t. \ \ \ y\_i(w^T x\_i+b) \geq 1 ,i=1,2, \cdots,n
\end{aligned}
\tag{9}\label{9}
$$

式子$\eqref{9}$就是线性可分支持向量机的基本模型。注意，前面加个$\frac{1}{2}$主要是为了方便后面求导，在机器学习算法中，很多地方都有用到这种做法，如逻辑回归。对于上式，如果求出了最优解$w^{\*}$ 和$b^{\*} $，就可以得到分离超平面$w^* x+b = 0$以及用于分类的决策函数$f(x)$：

$$ f(x) = sign({w^\*}  x+b^*) \tag{10}\label{10}$$

这里的$sign$表示的是符号函数。

###  支持向量

在上面的推导中，提到了几何间隔，也就是分离超平面$\mathbb{L}$与样本点之间的距离。在这里，我们称**距离分离超平面$\mathbb{L}$最近的训练数据样本点为支持向量**。即使公式$\eqref{9}$中约束条件等号成立（$y\_i (w^T x\_i + b) = \pm 1$）的点。而真正意义上的间隔$margin$，其实是支持向量到超平面$\mathbb{L}$距离的两倍$\frac{2}{||w||}$，也就是正类支持向量到负类支持向量的间隔。

> 提示：由于SVM的最大化间隔是最大化正负支持向量的距离，因此只依赖于支持向量，当数据存在噪声数据时，往往印象比较大，因为如果噪声数据恰好是支持向量，那结果必定收到很大的影响。因此，我们可以说SVM对噪声敏感。


## 线性SVM与对偶

通过上面的演算，我们得到了线性SVM的原始优化问题$\eqref{9}式$，那么我们应该如何求解该凸二次规划问题（convex quadratic programing）呢？方法有两种：

1. 使用现成的优化计算包求解，比如 MATLAB、LINGO等；
2. 将原始问题转化为对偶问题，进而求解最优解。

对于第一种方法，可以查查相关软件的API即可，本文主要介绍第二种方法，也是比较核心的技术。那么使用对偶的好处是什么呢？

1. 对偶形式比原始问题更容易求解；
2. 方便引入核函数，将数据推到更高维度，使数据显得线性可分。

下面使用对偶来求解优化问题$\eqref{9}$式。

### 对偶 & KKT

关于拉格朗日对偶（Lagrangian dual），本文不打算长篇大论，有疑问的可以参考[An Introduction to Duality in Convex Optimization.](https://www.net.in.tum.de/fileadmin/TUM/NET/NET-2011-07-2/NET-2011-07-2_20.pdf)和[Wiki-Duality-Optimization](https://en.wikipedia.org/wiki/Duality_(optimization))。本文侧重于如何运用拉格朗日对偶来解决凸优化问题。下面我们根据对偶来求解$\eqref{9}$式这个凸优化问题：

首先根据拉格朗日乘子法原理得到$\eqref{9}$式（称作原始问题）的拉格朗日表达式$L(w,b,\lambda)$，

$$ 
L(w, b, \lambda) = \frac{1}{2} ||w||^{2} + \sum\_{i=1}^{n} \lambda\_i (1- y\_i(w^T x\_i+b))  , \ \ \  i=1,2, \cdots,n
\tag{11} \label{11}$$

其中$\lambda = \lbrace \lambda\_1, \lambda\_2, \cdots, \lambda\_n \rbrace$，且 $\lambda\_i \geq 0$，俗称拉格朗日系数。对于$\eqref{11}式$，可以简单地分析下：由于$1- y\_i(w^T x\_i+b) \leq 0$，所以当$\lambda\_i <０$时，$\underset{\lambda\_i < 0}{max} \ L(w, b, \lambda) $的值将趋于$+\infty$，反之，当满足约束条件$\lambda \geq ０$时，$\underset{\lambda\_i \geq 0}{max} \ L(w, b, \lambda) $则恰巧等于$\eqref{9}$式的目标函数$\frac{1}{2} ||w||^2$。因此当最小化 $\underset{\lambda\_i \geq 0}{max} \ L(w, b, \lambda) $ 时，求解得到的最优解就是$\eqref{9}$式产生的最优解$p^\*$。如此一来，便将原始问题最优化问题表示成了广义拉格朗日函数的极小极大问题。使用数学公式可以表达成如下形式：

$$
\underset{w,b}{min} \  \underset{\lambda\_i \geq 0}{max} \ L(w, b, \lambda)  = \underset{w,b}{min} \ \frac{1}{2} ||w||^2 = p^\*
\tag{12}\label{12}$$

到了此步，由于求解最小最大问题难求较大，所以可以使用对偶原理将最小最大问题转换为最大最小问题。于是便得到原始问题的对偶问题是$\underset{\lambda}{max} \ \underset{w,b}{min} \ L(w,b,\lambda)$，其中$\lambda \geq ０$。

$$
\begin{aligned}
&\underset{\lambda}{max} \ \underset{w,b}{min} \ L(w,b,\lambda) \\\\
&s.t. \ \lambda\_i \geq 0 
\end{aligned}
\tag{13} \label{13}$$


对于式子$\eqref{13}$，可以先求出$\underset{w,b}{min} \ L(w,b,\lambda)$消除参数$w,b$，方法是对$L$中的 $w$和$b$求偏导并令偏导数等于零，可得$\eqref{14}$式与$\eqref{15}$式两个结果：

$$
\begin{aligned}
&\frac{\partial L(w,b,\lambda)}{\partial w} = w - \sum\_{i=1}^n \lambda\_iy\_ix\_i = 0 \\
&\frac{\partial L(w,b,\lambda)}{\partial b} = \sum\_{i=1}^n \lambda\_iy\_i = 0
\end{aligned}
$$

$$
w = \sum\_{i=1}^n \lambda\_i y\_i x\_i
\tag{14} \label{14}$$

$$
\sum\_{i=1}^n \lambda\_i y\_i =0
\tag{15} \label{15}$$


然后将上面两个等式代入$\eqref{13}$式中，便消除了$w,b$，得到下列优化方程（由于将式$\eqref{15}$代入到式$\eqref{13}$，因此约束条件需加入式$\eqref{15}$）：

$$
\begin{aligned}
\underset{\lambda}{max} \ &-\frac{1}{2} \sum\_{i=1}^n \sum\_{j=1}^n \lambda\_i \lambda\_j y\_iy\_j \left \langle x\_i, x\_j \right \rangle  + \sum\_{i=1}^n \lambda\_i \\\\
 s.t. \  &\sum\_{i=1}^n \lambda\_i y\_i = 0 \\\\
 &\lambda\_i \geq 0, i=1,2, \cdots, n 
\end{aligned}
\tag{16}\label{16}$$

最后，为了便于求解，还可以将$\eqref{16}$式中的目标函数添加负号，将$max$转为$min$求极小值，便得到下列对偶优化问题：

$$
\begin{aligned}
\underset{\lambda}{min} \ &\frac{1}{2} \sum\_{i=1}^n \sum\_{j=1}^n \lambda\_i \lambda\_j y\_iy\_j \left \langle x\_i, x\_j \right \rangle  - \sum\_{i=1}^n \lambda\_i \\\\
 s.t. \  &\sum\_{i=1}^n \lambda\_i y\_i = 0 \\\\
 &\lambda\_i \geq 0, i=1,2, \cdots, n 
\end{aligned}
\tag{17}\label{17}$$

对于式子$\eqref{17}$，记最优解为$d^\* $，由[KKT条件](http://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions)可以得出，$d^\* = p^\*$。因此，只需求出该表达式的最优解即可。

求解：可以通过求出此时的$\lambda$（此结果中，所有非支持向量$x\_i$的系数 $\lambda\_i$ 都等于零），然后将其代入到式子$\eqref{14}$求得$w^\*$。至于$b^\*$，可以根据支持向量$x\_j$的特点求出。我们知道，对于支持向量来说，其系数$\lambda\_j > 0$，且$y\_j(w^\*x + b^\* )= 1$，虽然支持向量有正负之分，但$y^2 = 1$是毫无争议的，因此可以求得出$b^\* = y\_j - \sum\_{i=1}^n \lambda\_i y\_i x\_i$。根据[KKT条件](http://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions)可知，此解，正是原问题的解。即最后的结果为：

$$
w^\* = \sum\_{i=1}^n \lambda\_i y\_i x\_i
\tag{18}\label{18}$$

$$
b^\* = y\_j - \sum\_{i=1}^n \lambda\_i y\_i x\_i
\tag{19}\label{19}$$

由此得到最终的决策函数为

$$
 f(x) = sign(\sum\_{i=1}^n \lambda\_i y\_i \left \langle x\_i, x \right \rangle  + b^\*)
\tag{20}\label{20}$$


刚刚说到，非支持向量的$\lambda$都等于$0$，其实是有原因的。根据[KKT条件](http://en.wikipedia.org/wiki/Karush%E2%80%93Kuhn%E2%80%93Tucker_conditions)，推导过程中会满足下列约束条件，即：

$$
\begin{aligned}
&\lambda\_i \geq 0 , \\\\
&y\_i (w^T x + b) -1 \geq 0  ,\\\\ 
&\lambda\_i [y\_i (w^T x + b) -1] = 0.
\end{aligned}
\tag{21}\label{21}$$

根据上式可以看出，当样本点非支持向量时，$y\_i (w^T x + b) -1 > 0$，然后根据第一个与第三个式子可以得出$\lambda\_i = 0$，即非支持向量的$\lambda$都恒等于$0$；另外，当样本点为支持向量时，$y\_i (w^T x + b) -1 = 0$，因为支持向量的函数间隔在前面固定为$1$了。

OK，上面介绍了使用对偶来求解线性可分情形下的凸优化问题$\eqref{9}$式，然而对于一些数据是线性不可分的或是存在噪声数据，这种情况该如何处理呢？下面介绍两种技术处理这种问题。

1. Soft Margin 
2. Kernel skill

## 软间隔与损失函数

### 软间隔理论

软间隔理论主要是为了兼容噪声数据，由于这些噪声数据改变了原数据的线性结构从而致使其变成非线性结构。因此，为了兼容这些数据点，可以为每个样本点$x\_i$引入一个松弛变量$\xi\_i \geq 0$，使得$y\_i(w^T x\_i+b) \geq 1  - \xi\_i$，从而可以将$\eqref{9}$式化为下式：


$$ 
\begin{aligned}
\underset{w,b}{min} \ \ &\frac{1}{2} ||w||^{2} \color{red}{ + C \sum\_{i=1}^n \xi\_i}  \\\\ 
s.t. \ \ \ &y\_i(w^T x\_i+b) \geq 1 \color{red}{ - \xi\_i}, i=1,2, \cdots,n    \\\\ 
&\xi\_i \geq 0,  i=1,2, \cdots,n
\end{aligned}
\tag{22}\label{22}
$$

对于式子$\eqref{22}$，也可以通过对偶最终可将其变换成如下问题（过程与上面的类似，在此就省略了）：

$$
\begin{aligned}
\underset{\lambda}{min} \ &\frac{1}{2} \sum\_{i=1}^n \sum\_{j=1}^n \lambda\_i \lambda\_j y\_iy\_j \left \langle x\_i, x\_j \right \rangle  - \sum\_{i=1}^n \lambda\_i \\\\
 s.t. \  &\sum\_{i=1}^n \lambda\_i y\_i = 0 \\\\
 & 0 \leq \lambda\_i \color{red}{ \leq C}, i=1,2, \cdots, n 
\end{aligned}
\tag{23}\label{23}$$

将$\eqref{23}$式与$\eqref{17}$式对比发现，唯一的区别就是在对偶变量$\lambda$ 上加了一个上限 $C$。最终，在约束条件下，求解出$w^\*$与$b^\*$即可。

### 损失函数与正则化

刚刚说到，公式$\eqref{22}$是通过加入软间隔之后得到的，然而可以通过损失函数与正则化来理解它。在最大化间隔的时候，我们希望不满足约束条件$y\_i(w^T x\_i+b) \geq 1$的样本点应该尽量的少，这样可以减弱噪声数据的影响。因此，当引入$l\_{0/1}$(0-1)损失之后，目标函数可以表达成下列式子：

$$
\underset{w,b}{min} \frac{1}{2} ||w||^2 + C \sum\_{i=1}^{n} l\_{0/1}(y\_i(w^T x\_i+b) - 1)
\tag{24}\label{24}$$

根据[0-1损失](http://www.cs.cmu.edu/~yandongl/loss.html)的性质可以得到，当$y\_i(w^T x\_i+b) - 1 \geq 0$，$l\_{0/1} = 0$，化简后刚好可以得到$\eqref{9}$式。 然而，0-1损失函数是非凸函数，并且不连续，造成$\eqref{24}$式难以求解，因此需要使用其他的函数进行替换。而SVM采用了典型的hinge损失函数$l\_{hinge}(x) = max(0, 1-x)$，于是$\eqref{24}$式可化简为：

$$
\underset{w,b}{min} \frac{1}{2} ||w||^2 + C \sum\_{i=1}^{n} \color{red}{max(0, 1 - y\_i(w^T x\_i+b))}
\tag{25}\label{25}$$

对于式$\eqref{25}$，当$1 - y\_i(w^T x\_i+b) \leq 0$时，则$max(0, 1 - y\_i(w^T x\_i+b) ) = 0$，式子化简后也可得到$\eqref{9}$式结果。当加入松弛变量$\xi\_i$之后，就可得到$\eqref{22}$式的结果。

> **解释**：令$\xi\_i = max(0, 1 - y\_i(w^T x\_i+b)$，就得到了式$\eqref{22}$的目标式子，当$1 - y\_i(w^T x\_i+b) \leq 0$时，$\xi\_i = 0 \geq 1 - y\_i(w^T x\_i+b)$；当$1 - y\_i(w^T x\_i+b) > 0$，$\xi\_i = 1 - y\_i(w^T x\_i+b) > 0$。因此，$\xi\_i$始终是大于等于$0$的，并且$\xi\_i \geq 1 - y\_i(w^T x\_i+b)$，化简后等于$y\_i(w^T x\_i+b) \geq 1 - \xi\_i$，即$\eqref{22}$式的两个约束条件。

经过刚刚的分析，可以知道式$\eqref{25}$的后半部分为损失函数，那前半部分是什么呢？没错，就是$L2$正则项。从而得出，SVM的目标表达式其实就是最小化损失函数与正则项两者的和。从损失函数的角度来看，hinge损失函数在大于$1$的区域都为$0$，这使得支持向量的解具有稀疏性。从正则化的角度来看，有助于削减假设空间，从而降低过拟合风险。另外，如果将$\eqref{24}$式的0-1损失替换成log损失，其目标函数与逻辑回归的表达式就类似了。

## 非线性SVM与核函数

对于线性问题或是存在部分噪声的非线性问题，使用线性SVM加上软间隔已经非常有效了。但在实际当中，很多问题都是非线性的，这就需要额外的处理技巧来解决这类问题，也就是接下来提到的核函数。不得不说，kernel的使用不止于SVM，只是在SVM中表现得非常突出罢了。由于笔者能力有限，所以不打算将kernel method的内容详细地讲解，有兴趣的同学可以参考[Kernel_method](https://en.wikipedia.org/wiki/Kernel_method)以及[SVM_Seminarbericht_Hofmann](http://www.cogsys.wiai.uni-bamberg.de/teaching/ss06/hs_svm/slides/SVM_Seminarbericht_Hofmann.pdf)，另外pluskid的[SVM kernel](http://blog.pluskid.org/?p=685) 也讲解的比较好。

> 对于核技巧，简而言之，就是在数据线性不可分的情况下，通过某种事先选择的非线性映射（核函数）将输入样本映射到一个高维的特征空间，然后在这个空间中构造最优的分类超平面。

核的引入其实是有迹可循的，只要表达式中存在内积，都可使用核函数将其映射到高维空间中。对于式$\eqref{17}$和式$\eqref{23}$，都存在$\left \langle x\_i, x\_j \right \rangle$这样的内积，所以都可以使用核函数。这里，以式子$\eqref{23}$为例，使用核函数$K(x\_1, x\_2)$后，可将其变为下式：


$$
\begin{aligned}
\underset{\lambda}{min} \ &\frac{1}{2} \sum\_{i=1}^n \sum\_{j=1}^n \lambda\_i \lambda\_j y\_iy\_j \color{red}{K( x\_i, x\_j)}  - \sum\_{i=1}^n \lambda\_i \\\\
 s.t. \  &\sum\_{i=1}^n \lambda\_i y\_i = 0 \\\\
 & 0 \leq \lambda\_i \color{red}{ \leq C}, i=1,2, \cdots, n 
\end{aligned}
\tag{26}\label{26}$$


好了，下面归纳一下常用的核函数。

### 常用核函数

|名称|公式| 意义|
|----|----| -- |
|线性核|$$K(x\_1, x\_2) = \left \langle x\_1, x\_2  \right \rangle$$| - |
|多项式核|$$K(x\_1, x\_2) = ( \left \langle x\_1, x\_2  \right \rangle + R)^d$$| - |
|高斯核|$$K(x\_1, x\_2) = exp \left \(−\dfrac{∥x\_1− x\_2∥^2}{ \sigma \_2 } \right \)$$| 映射维度与参数$\sigma$有关，当$\sigma$比较大时，会得到一个低维子空间，当$\sigma$比较小时，会将输入空间映射至无穷维 |


## SMO算法

对于线性可分和线性不可分情形都有了对应的策略，通过dual 原理化简之后，最终分别可以得到$\eqref{17}$式与$\eqref{23}$式。这两个式子都只存在$n$个$\lambda$变量，即$\lambda\_1, \lambda\_2, \cdots, \lambda\_n$，只需要将这些$\lambda$求解出就可以带入到$\eqref{18}$式与$\eqref{19}$式求出最终的$w^\*$和$b^\*$。通常对于一般的二次方程式，我们可以使用[Gradient Descent](http://en.wikipedia.org/wiki/Gradient_descent)、[Coordinate Descent](https://en.wikipedia.org/wiki/Coordinate_descent)、[Newton's Method](https://en.wikipedia.org/wiki/Newton%27s_method)等优化方法来求解，但这里存在约束条件，那么如何在约束条件下求解最优的$\lambda$呢？也就是这节要提到的SMO（sequential minimal optimization）序列最小最优化算法。






有关SMO更多的内容，可参考[Sequential Minimal Optimization: A Fast Algorithm for Training Support Vector Machines](http://luthuli.cs.uiuc.edu/~daf/courses/optimization/Papers/smoTR.pdf)。




## 总结

### 关于SVM的一些知识点

SVM包含的知识点比较多，对于不同的数据有不同的学习策略：

- 当数据是线性可分的时候，他通过最大化硬间隔学习一个线性分类器，即线性可分支持向量机；
- 当训练数据近似线性可分时，他通过最大化软间隔学习一个线性分类器，即软间隔支持向量；
- 当训练数据线性不可分时，通过使用核函数及软间隔最大化，学习非线性支持向量。
  
欧式空间：（欧几里德空间E^n ）在n维实向量空间R^n 中定义内积(x,y)=x1y1+...+xnyn，则R^n 为欧几里德空间。（事实上，任意一个n维欧几里德空间V等距同构于E^n。）———平面性。


当输入空间为欧式空间或离散集合、特征空间为希尔伯特空间时，<font color="#007FFF">**核函数表示将输入和输入空间映射到特征空间得到的特征向量的内积**</font>。

## References

- [optimization - ppt](https://www.princeton.edu/~chiangm/optimization.pdf)
- [pluskid-支持向量系列](http://blog.pluskid.org/?page_id=683)
- [Wiki-Duality-Optimization](https://en.wikipedia.org/wiki/Duality_(optimization))
- Wolf, Stephan. "[An Introduction to Duality in Convex Optimization.](https://www.net.in.tum.de/fileadmin/TUM/NET/NET-2011-07-2/NET-2011-07-2_20.pdf)" Network 153 (2011).

