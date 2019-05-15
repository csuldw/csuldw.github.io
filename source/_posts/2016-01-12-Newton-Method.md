---
layout: post
date: 2016-01-12 10:24
title: "机器学习-牛顿方法&指数分布族&GLM"
categories: ML
tag: 
	- 牛顿方法
	- 指数分布族
	- GLM
	- Machine Learning
comment: true
---


<font color="green">回头再温习一下Andrew Ng的机器学习视频课，顺便把没写完的笔记写完。</font>

本节内容

- 牛顿方法
- 指数分布族
- 广义线性模型

<!-- more -->

之前学习了梯度下降方法，关于梯度下降（gradient descent），这里简单的回顾下【参考感知机学习部分提到的梯度下降([gradient descent](http://blog.csdn.net/dream_angel_z/article/details/48915561))】。在最小化损失函数时，采用的就是梯度下降的方法逐步逼近最优解，规则为$\theta := \theta - \eta \nabla_{\theta} \ell(\theta)$。其实梯度下降属于一种优化方法，但梯度下降找到的是局部最优解。如下图：

<center>
![这里写图片描述](http://img.blog.csdn.net/20151006094200515)
</center>


本节首先讲解的是牛顿方法（NewTon's Method）。牛顿方法也是一种优化方法，它考虑的是**全局最优**。接着还会讲到指数分布族和广义线性模型。下面来详细介绍。

## **1.牛顿方法**

现在介绍另一种最小化损失函数$\ell(\theta)$的方法——牛顿方法,参考[Approximations Of Roots Of Functions – Newton's Method](http://www.phengkimving.com/calc_of_one_real_var/08_app_of_the_der_part_2/08_05_approx_of_roots_of_func_newtons_meth.htm)
。它与梯度下降不同，其基本思想如下：

假设一个函数$f(x) = 0$,我们需要求解此时的$x$值。如下图所示：

<center>
![这里写图片描述](http://img.blog.csdn.net/20151006095845371)
图1 $f(x0) = 0, a1, a2, a3, ... 逐步接近 x0$.
</center>

在
$a_1$点的时候，$f(x)$切线的目标函数$y = f(a_1) + f '(a_1)(x – a_1)$. 由于$(a_2,0)$在这条线上，所以我们有$ 0 = f(a_1) + f '(a_1)(a_2 – a_1)$,so:

$$a_2 = a_1-\frac{f(a_1)}{f'(a_1)}$$

同理，在$a_2$点的时候，切线的目标函数$y = f(a_2) + f '(a_2)(x – a_2)$. 由于$(a_3,0)$在这条线上，所以我们有$ 0 = f(a_2) + f '(a_2)(a_3– a_2)$,so:

$$a_3 = a_2-\frac{f(a_2)}{f'(a_2)}$$

假设在第$n$次迭代，有$f(a_n)=0$,那么此时有下面这个递推公式：

![](http://latex.codecogs.com/gif.latex?%24%24a_n%20%3D%20a_%7Bn-1%7D-%5Cfrac%7Bf%28a_%7Bn-1%7D%29%7D%7Bf%27%28a_%7Bn-1%7D%29%7D%24%24)

其中$n>=2$.

最后得到的公式也就是牛顿方法的学习规则，为了和梯度下降对比，我们来替换一下变量，公式如下：

$$\theta := \theta - \frac{f(\theta)}{f'(\theta)}$$


<font color="green">**那么问题来了，怎么将牛顿方法应用到我们的问题上，最小化损失函数$\ell(\theta)$(或者是求极大似然估计的极大值)呢？**
</font>

  对于机器学习问题，现在我们优化的目标函数为极大似然估计$\ell$，当极大似然估计函数取值最大时，其导数为 0，这样就和上面函数f取 0 的问题一致了，令$f(\theta) = \ell'(\theta)$。极大似然函数的求解更新规则是：
  
$$\theta := \theta - \frac{\ell'(\theta)}{\ell''(\theta)}$$

对于$\ell$，当一阶导数为零时，有极值；此时，如果二阶导数大于零，则$\ell$有极小值，如果二阶导数小于零，则有极大值。

上面的式子是当参数$\theta$为实数时的情况，下面我们要求出一般式。当参数为向量时，更新规则变为如下公式：

$$\theta := \theta - H^{-1} \nabla_{\theta}\ell(\theta)$$

其中$\nabla$后半部分$和之前梯度下降中提到的一样，是梯度，$H$是一个$n*n$的矩阵，$H $是函数的二次导数矩阵，被成为$Hessian$矩阵。其某个元素$ H_{ij}$ 计算公式如下：

![$$H_{ij}=\dfrac{\partial^{2}\ell(\theta)}{\partial\theta_{i}\theta_{j}}$$](http://latex.codecogs.com/gif.latex?%24%24H_%7Bij%7D%3D%5Cdfrac%7B%5Cpartial%5E%7B2%7D%5Cell%28%5Ctheta%29%7D%7B%5Cpartial%5Ctheta_%7Bi%7D%5Ctheta_%7Bj%7D%7D%24%24)

<font color="red">**和梯度下降相比，牛顿方法的收敛速度更快，通常只要十几次或者更少就可以收敛，牛顿方法也被称为二次收敛（quadratic convergence），因为当迭代到距离收敛值比较近的时候，每次迭代都能使误差变为原来的平方。缺点是当参数向量较大的时候，每次迭代都需要计算一次 Hessian 矩阵的逆，比较耗时。**
</font>

## **2.指数分布族（The exponential family）**


指数分布族是指可以表示为指数形式的概率分布。指数分布的形式如下：

$$P(y;\eta)=b(y)exp(\eta^{T}T(y)-a(\eta))$$

其中，η成为分布的**自然参数**（nature parameter）；T(y)是**充分统计量**（sufficient statistic），通常 **T(y)=y**。当参数 a、b、T 都固定的时候，就定义了一个以η为参数的函数族。

下面介绍两种分布，伯努利分布和高斯分布，分别把它们表示成指数分布族的形式。

### **伯努利分布**

伯努利分布是对0，1问题进行建模的，对于Bernoulli（$\varphi$）,$y\epsilon\{0, 1\}$.有$p(y=1; \varphi ) = \varphi; p(y=0; \varphi ) = 1- \varphi$，下面将其推导成指数分布族形式：

<center>
![这里写图片描述](http://img.blog.csdn.net/20151006105233062)
</center>

将其与指数族分布形式对比，可以看出：

<center>
![这里写图片描述](http://img.blog.csdn.net/20151006105357353)
</center>

表明伯努利分布也是指数分布族的一种。从上述式子可以看到，$\eta$的形式与logistic函数（sigmoid）一致，这是因为 logistic模型对问题的前置概率估计其实就是伯努利分布。

### **高斯分布**
下面对高斯分布进行推导，推导公式如下（为了方便计算，我们将方差 $\sigma$设置为1）：

<center>
![这里写图片描述](http://img.blog.csdn.net/20151006105614842)
</center>

将上式与指数族分布形式比对，可知：

$$b(y) = \frac{1}{\sqrt{2\pi}}exp(-\frac{1}{2}y^{2})$$

$$T(y) = y$$

$$\eta = \mu$$

$$a(\eta)=\frac{1}{2}\mu^{2}$$

两个典型的指数分布族，伯努利和高斯分布。其实大多数概率分布都可以表示成指数分布族形式，如下所示：

- 伯努利分布（Bernoulli）：对 0、1 问题进行建模；
- 多项式分布（Multinomial）：多有 K 个离散结果的事件建模；
- 泊松分布（Poisson）：对计数过程进行建模，比如网站访问量的计数问题，放射性衰变的数目，商店顾客数量等问题；
- 伽马分布（gamma）与指数分布（exponential）：对有间隔的正数进行建模，比如公交车的到站时间问题；
- β 分布：对小数建模；
- Dirichlet 分布：对概率分布进建模；
- Wishart 分布：协方差矩阵的分布；
- 高斯分布（Gaussian）；

下面来介绍下广义线性模型（Generalized Linear Model, GLM）。

## **3.广义线性模型（Generalized Linear Model, GLM）**

你可能会问，指数分布族究竟有何用？其实我们的目的是要引出GLM，通过指数分布族引出广义线性模型。

仔细观察伯努利分布和高斯分布的指数分布族形式中的$\eta$变量。可以发现，在伯努利的指数分布族形式中，$\eta$与伯努利分布的参数$\varphi$是一个logistic函数（下面会介绍logistic回归的推导）。此外，在高斯分布的指数分布族表示形式中，$\eta$与正态分布的参数$\mu$相等，下面会根据它推导出普通最小二乘法（Ordinary Least Squares）。通过这两个例子，我们大致可以得到一个结论，<font color="red">**$η$以不同的映射函数与其它概率分布函数中的参数发生联系，从而得到不同的模型，广义线性模型正是将指数分布族中的所有成员（每个成员正好有一个这样的联系）都作为线性模型的扩展，通过各种非线性的连接函数将线性函数映射到其他空间，从而大大扩大了线性模型可解决的问题。**</font>

下面我们看 GLM 的形式化定义，GLM 有三个假设：

- (1) $y|x; \theta~ExponentialFamily（\eta）$；给定样本$ x $与参数$θ$，样本分类$ y$ 服从指数分布族中的某个分布；
- (2) 给定一个 $x$，我们需要的目标函数为$h_{\theta}(x)=E[T(y)|x]$;
- (3)$\eta=\theta^{T}x$。

依据这三个假设，我们可以推导出logistic模型与普通最小二乘模型。首先根据伯努利分布推导Logistic模型，推导过程如下:

$$h_{\theta}(x) = E[T(y)|x]=E[y|x]=p(y=1|x;\theta)$$

$$=\varphi$$

$$=\frac{1}{1+e^{-\eta}}$$

$$=\frac{1}{1+e^{-\theta^{T}x}}$$

公式第一行来自假设(2)，公式第二行通过伯努利分布计算得出，第三行通过伯努利的指数分布族表示形式得出，然后在公式第四行，根据假设三替换变量得到。

同样，可以根据高斯分布推导出普通最小二乘，如下：

$$h_{\theta}(x) = E(T(y)|x)=E[y|x]$$

$$=\mu$$

$$=\eta$$

$$=\theta^{T}x$$

公式第一行来自假设（2），第二行是通过高斯分布$y|x;\theta$~$ N(\mu,\sigma^{2})$计算得出，第三行是通过高斯分布的指数分布族形式表示得出，第四行即为假设（3）。

其中，将η与原始概率分布中的参数联系起来的函数成为正则相应函数（canonical response function），如$φ =\frac{1}{1+e^{-\eta}}、μ = η$即是正则响应函数。正则响应函数的逆成为正则关联函数（canonical link function）。

所以，对于广义线性模型，需要决策的是选用什么样的分布，当选取高斯分布时，我们就得到最小二乘模型，当选取伯努利分布时，我们得到 logistic 模型，这里所说的模型是假设函数 h 的形式。

最后总结一下：<font color="red">**广义线性模型通过假设一个概率分布，得到不同的模型，而梯度下降和牛顿方法都是为了求取模型中的线性部分$(\theta^{T}x)$的参数$\theta$的。**</font>

**多分类模型-Softmax Regression**

下面再给出GLM的一个例子——**Softmax Regression**.

假设一个分类问题，y可取k个值，即$y \epsilon\{1,2,...,k\}$。现在考虑的不再是一个二分类问题，现在的类别可以是多个。如邮件分类：垃圾邮件、个人邮件、工作相关邮件。下面要介绍的是多项式分布（multinomial distribution）。

多项式分布推导出的GLM可以解决多类分类问题，是 logistic 模型的扩展。对于多项式分布中的各个y的取值，我们可以使用k个参数$\phi_1,\phi_2,...,\phi_k$来表示这k个取值的概率。即

$$P(y=i) = \phi_{i}$$

但是，这些参数可能会冗余，更正式的说可能不独立，因为$\sum\phi_i=1$，知道了前k-1个，就可以通过$1-\sum_{i=1}^{k-1}\phi_{i}$计算出第k个概率。所以，我们只假定前k-1个结果的概率参数$\phi_1$,$\phi_2$,...,$\phi_{k-1}$，第k个输出的概率通过下面的式子计算得出：

![$$\phi_{k} = 1- \sum_{i=1}^{k-1}\phi_{i}$$](http://latex.codecogs.com/gif.latex?%24%24%5Cphi_%7Bk%7D%20%3D%201-%20%5Csum_%7Bi%3D1%7D%5E%7Bk-1%7D%5Cphi_%7Bi%7D%24%24)

为了使多项式分布能够写成指数分布族的形式，我们首先定义 T(y)，如下所示：

<center>
![](http://img.blog.csdn.net/20151006125938706)
</center>

和之前的不一样，这里我们的$T(y)$不等$y$，$T(y)$现在是一个$k-1$维的向量，而不是一个真实值。接下来，我们将使用$(T(y))_{i}$表示$T(y)$的第i个元素。

下面我们引入指数函数I，使得：

$$I(True)=1,I(False)=0$$

这样，$T(y)$向量中的某个元素还可以表示成：

$$(T(y))_{i}=I(y=i)$$

举例来说，当$ y=2 时，T(2)_2=I(2=2)=1，T(2)_3=I(2=3)=0$。根据公式 15，我们还可以得到：

![$$E[(T(y))_{i}]=\sum_{y=1}^{k}(T(y)){\phi}_i=\sum_{y=1}^{k}I(y=i)\phi_i=\phi_i$$](http://latex.codecogs.com/gif.latex?%24%24E%5B%28T%28y%29%29_%7Bi%7D%5D%3D%5Csum_%7By%3D1%7D%5E%7Bk%7D%28T%28y%29%29%7B%5Cphi%7D_i%3D%5Csum_%7By%3D1%7D%5E%7Bk%7DI%28y%3Di%29%5Cphi_i%3D%5Cphi_i%24%24)

$$\sum_{i=1}^{k}I(y=i)=1$$

下面，二项分布转变为指数分布族的推导如下：

<center>
![这里写图片描述](http://img.blog.csdn.net/20151006131133675)
</center>

其中，最后一步的各个变量如下：

<center>
![这里写图片描述](http://img.blog.csdn.net/20151006131218488)
</center>

由$\eta$的表达式可知：

![$$\eta_{i}=log\frac{\phi_{i}}{\phi_{k}}\Rightarrow \phi_{i}=\phi_{k}e^{\eta_{i}}$$](http://latex.codecogs.com/gif.latex?%24%24%5Ceta_%7Bi%7D%3Dlog%5Cfrac%7B%5Cphi_%7Bi%7D%7D%7B%5Cphi_%7Bk%7D%7D%5CRightarrow%20%5Cphi_%7Bi%7D%3D%5Cphi_%7Bk%7De%5E%7B%5Ceta_%7Bi%7D%7D%24%24)

为了方便，再定义：

![$$\eta_{k} = log \frac{\phi_{k}}{\phi_{k}}=0$$](http://latex.codecogs.com/gif.latex?%24%24%5Ceta_%7Bk%7D%20%3D%20log%20%5Cfrac%7B%5Cphi_%7Bk%7D%7D%7B%5Cphi_%7Bk%7D%7D%3D0%24%24)

于是，可以得到：

![$$\sum_{j=1}^{k}\phi_{i}=\sum_{j=1}^{k}\phi_{k}e^{\eta_{i}}=1 \Rightarrow \phi_{k}=\frac{1}{\sum_{j=1}^{k}e^{\eta_{i}}}$$](http://latex.codecogs.com/gif.latex?%24%24%5Csum_%7Bj%3D1%7D%5E%7Bk%7D%5Cphi_%7Bi%7D%3D%5Csum_%7Bj%3D1%7D%5E%7Bk%7D%5Cphi_%7Bk%7De%5E%7B%5Ceta_%7Bi%7D%7D%3D1%20%5CRightarrow%20%5Cphi_%7Bk%7D%3D%5Cfrac%7B1%7D%7B%5Csum_%7Bj%3D1%7D%5E%7Bk%7De%5E%7B%5Ceta_%7Bi%7D%7D%7D%24%24)

将上式代入到

![$$\eta_{i}=log\frac{\phi_{i}}{\phi_{k}}\Rightarrow \phi_{i}=\phi_{k}e^{\eta_{i}}$$](http://latex.codecogs.com/gif.latex?%5Ceta_%7Bi%7D%3Dlog%5Cfrac%7B%5Cphi_%7Bi%7D%7D%7B%5Cphi_%7Bk%7D%7D%5CRightarrow%20%5Cphi_%7Bi%7D%3D%5Cphi_%7Bk%7De%5E%7B%5Ceta_%7Bi%7D%7D)，得到：

![$$\phi_{i}=\frac{e^{\eta_{i}}}{\sum_{j=1}^{k}e^{\eta_{i}}}=\frac{e^{\eta_{i}}}{1+\sum_{j=1}^{k-1}e^{\eta_{i}}}$$](http://latex.codecogs.com/gif.latex?%24%24%5Cphi_%7Bi%7D%3D%5Cfrac%7Be%5E%7B%5Ceta_%7Bi%7D%7D%7D%7B%5Csum_%7Bj%3D1%7D%5E%7Bk%7De%5E%7B%5Ceta_%7Bi%7D%7D%7D%3D%5Cfrac%7Be%5E%7B%5Ceta_%7Bi%7D%7D%7D%7B1&plus;%5Csum_%7Bj%3D1%7D%5E%7Bk-1%7De%5E%7B%5Ceta_%7Bi%7D%7D%7D%24%24)

从而，我们就得到了连接函数，有了连接函数后，就可以把多项式分布的概率表达出来:

![$$P(y=i)=\phi_{i}=\frac{e^{\eta_{i}}}{1+\sum_{j=1}^{k-1}e^{\eta_{i}}}=\frac{e^{\theta_{i}^{T}x}}{1+\sum_{j=1}^{k-1}e^{\theta_{j}^{T}x}}$$](http://latex.codecogs.com/gif.latex?%24%24P%28y%3Di%29%3D%5Cphi_%7Bi%7D%3D%5Cfrac%7Be%5E%7B%5Ceta_%7Bi%7D%7D%7D%7B1&plus;%5Csum_%7Bj%3D1%7D%5E%7Bk-1%7De%5E%7B%5Ceta_%7Bi%7D%7D%7D%3D%5Cfrac%7Be%5E%7B%5Ctheta_%7Bi%7D%5E%7BT%7Dx%7D%7D%7B1&plus;%5Csum_%7Bj%3D1%7D%5E%7Bk-1%7De%5E%7B%5Ctheta_%7Bj%7D%5E%7BT%7Dx%7D%7D%24%24)

注意到，上式中的每个参数$\eta_i$都是一个可用线性向量$\theta_i^Tx$表示出来的，因而这里的$\theta$其实是一个二维矩阵。

于是，我们可以得到假设函数 h 如下：

<center>
![这里写图片描述](http://img.blog.csdn.net/20151006132537928)
</center>

那么就建立了假设函数，最后就获得了最大似然估计 

<center>
![这里写图片描述](http://img.blog.csdn.net/20151006132628356)
</center>

对该式子可以使用梯度下降算法或者牛顿方法求得参数$\theta$后，使用假设函数$h$对新的样例进行预测，即可完成多类分类任务。这种多种分类问题的解法被称为 softmax regression.


## **References**

- [Approximations Of Roots Of Functions – Newton's Method](http://www.phengkimving.com/calc_of_one_real_var/08_app_of_the_der_part_2/08_05_approx_of_roots_of_func_newtons_meth.htm)
- 机器学习-Andrew Ng 斯坦福大学[机器学习视频-第四讲](http://open.163.com/movie/2008/1/E/D/M6SGF6VB4_M6SGHKAED.html)

---
<center><strong>本栏目机器学习持续更新中，欢迎来访：<a href="http://blog.csdn.net/dream_angel_z">Dream_Angel_Z 博客</a>
新浪微博： <a href="http://weibo.com/liudiwei210" target="_black">@拾毅者</a><br>
</strong></center>