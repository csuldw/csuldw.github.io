---
layout: post
date: 2016-08-23 09:24
title: "Computational Learning Theory - VC Dimension"
categories: ML
tag: 
	- VC dimension
	- Machine Learning
	- PAC
	- Hoeffding Inequality
comment: true
---

在计算学习理论里面，有一个比较重要的概念，那就是VC维（Vapnic-Chervonenkis Dimension）。它解释了机器学习算法为什么可以去学习，数据又为什么可以被学习。在机器学习领域，VC维可以说是一个非常基础的定量化概念，可用来刻画分类系统的性能，也因此给诸多机器学习方法的可学习性提供了坚实的理论基础。网上有许多讲解VC维的博文，自己在学习VC维的时候也搜到很多，数量多的同时质量难免也良莠不齐。所以在这里，强烈推荐一下pluskid写的关于[VC theory](http://freemind.pluskid.org/slt/vc-theory-hoeffding-inequality/) 的系列文章，总结的确实非常深入。

<!-- more -->

由于pluskid的VC维系列比较深入，如果有一定的数学基础，那么学起来倒不难，但对初学者来说可能就比较困难。不过，不用担心，还有一份比较好的入门级学习教程，就是林轩田老师的《机器学习基石》，该课程前面几堂课讲的都是VC维，并且林老师讲解VC维时将理论与实例结合在一起，以讲故事的形式来阐述VC维，极力推荐一下，的确值得一听。有兴趣的同学可以去Google找找资料，这里推荐一个学者在听课过程中总结的笔记“[《机器学习基石课程笔记》](http://beader.me/mlnotebook/)“。然而，虽然目前已经有很多前辈都透彻地阐述了VC维，但要想将知识转为己有，就必须自己反复地去琢磨，去回顾归纳并去总结。本文对于VC维而言可以说是冰山一角，想要更全面地理解VC维，还请读者前去查阅与VC维有关的paper。此外，本文主要以总结为主，理论上的证明将不会过多的涉及，内容则主要围绕下面几点展开：

1. Hoeffding Inequality
2. Probably Approximately Correct Learnable
3. Vapnic-Chervonenkis Dimension
	- Growth function
	- Dichotomy
	- Shatter && Break point
	- VC dimension

最近，在总结VC维的时候，脑海中总会浮现以下几个问题，确切的说，这也是VC维能够解决的事情（当然，不同的人会有不同的想法，或者携带的问题会更多）：

1. 为什么机器学习方法可以学习？
2. 为什么会出现过拟合？
3. 机器学习模型的复杂度如何衡量？

下面根据上文提到的几点，来对各个要点逐个展开。

## Hoeffding Inequality

[Hoeffding不等式](https://en.wikipedia.org/wiki/Hoeffding%27s_inequality)是一个关于一组随机变量均值的概率不等式。

> **Hoeffding Inequality**：设$X\_1, X\_2,\cdot, X\_n$为一组独立同分布的随机变量，满足$X_i \epsilon [a, b], 0 \leq i \leq n $，$n$为随机变量的个数。记$n$个随机变量的经验期望 $\bar{X} = \frac{X_1 + X_2 + \cdots + X_n}{n}$，则对于任意的$0<\delta<1$, Hoeffding不等式可以表示为:

$$ P(\bar{X} - E(\bar{X}) \geq \epsilon   ) \leq exp \left \(\frac{-2 n \epsilon ^{2} }{(b-a)^{2}} \right \) \tag{1} \label{1}$$

在上述式子中，注意到$X\_i$的取值范围是$[a, b]$，当分类误差为$0-1$损失时，$a$和$b$分别对应到$0$和$1$，此时，方程$\eqref{1}$简化为

$$ P(\bar{X} - E(\bar{X}) \geq \epsilon   ) \leq exp\left\(-2 n \epsilon ^{2} \right \) \tag{2}$$

或

$$ P( \left \| \bar{X} - E(\bar{X}) \right \| \geq \epsilon   ) \leq 2  exp \left \(-2 n \epsilon ^{2} \right \) \tag{3} \label{3}$$

如果单纯的从概率的角度来看，Hoeffding 不等式刻画的是某个随机事件的真实概率及其$n$个独立重复试验中观察到的频率之间的差异。换言之，它是$n$个不同伯努利（Bernoulli）试验的应用。那么现在的问题是：**对于这样一个不等式，如何将其与机器学习联系在一起呢？**

首先，机器学习的目的是从合理数量的训练数据中，通过合理的计算量可靠的学习到知识。换句话说，就是从已有的数据中使用算法从假设空间$\mathcal{H}$中选择一个最好的$g$，虽然这个$g$很好，但它与样本真实的目标函数$f$存在一定的区别，从而导致训练时产生error。为了方便起见，我们将机器学习是在训练时会产生的训练误差叫做$E\_{in}(g)$，当样本量为$n$时（从某个data set中有放回的抽取$n$次，最终未抽到的样本为out-of-sample），其表达式为：

$$ E\_{in}(g) = \frac{1}{n} \sum\_{i=1}^{n}  l(g, X\_i, y_i) \tag{4}$$

类似地，我们把out-of-sample的产生的误差叫做$E\_{out}(g)$，表达式为

$$E\_{out}(g) = \mathbb{E}\_{P\_{xy}} \left \[ l(g, X, Y) \right \] \tag{5}$$

因此，可以将Hoeffding 不等式应用到$E\_{in}$和$E\_{out}$，公式$\eqref{3}$可化为式$\eqref{6}$

$$ P( \left \| E\_{in} - E\_{out} \right \| \geq \epsilon   ) \leq 2  exp \left \(-2 n \epsilon ^{2} \right \) \tag{6} \label{6}$$

公式$\eqref{6}$表达了算法学习到的$g$属于bad model的概率。换言之，当右边这个“upper bound”足够小时，我们可以说$g$在sample中的表现(错误率)与$g$在总体中的表现是差不多的。然而，对于假设空间$\mathcal{H}$中一个固定的$h$ 而言，$E\_{in}(h)$会与$E\_{out}(h)$很接近，这种情况能说是一种好的learning吗？答案当然是不能的，因为如果$E\_{in}(h)$很大，则$E\_{out}(h)$也大，这种学习得到的结果显然意义不大。

然而，不幸的是，在实际的采样过程中，我们可能会碰到bad sample（凡是由于抽样误差所造成样本分布与总体分布相差很大的样本，我们都可以称之为bad sample。）,从而造成训练时选择的$g$并非我们想要的。So，bad sample发生的概率有多大呢？当假设空间$\mathcal{H}$有$M$个可以学习的函数时，每个函数发生bad sample的概率为$\leq 2exp\left \( -2 \epsilon^2 n \right \)$，如果每个$M$都出现了bad sample，则出现bad sample总概率为：

$$  \begin{aligned} \mathbb{P}\_{\mathcal{D}}[BAD\ \mathcal{D}] 
& = \mathbb{P}\_{\mathcal{D}}[BAD\ \mathcal{D}\ for\ h\_1\ or\ BAD\ \mathcal{D}\ for\ h\_2\ or\ ...\ or\ BAD\ \mathcal{D}\ for\ h\_M]\\\
\ & \leq \mathbb{P}\_{\mathcal{D}}[BAD\ \mathcal{D}\ for\ h\_1] + \mathbb{P}\_{\mathcal{D}}[BAD\ \mathcal{D}\ for\ h\_2]+...+\mathbb{P}\_{\mathcal{D}}[BAD\ \mathcal{D}\ for\ h\_M] \\\
\ & \leq 2exp(-2\epsilon ^2n) + \leq 2exp(-2\epsilon ^2n) + ... + \leq 2exp(-2\epsilon ^2n) \\\
\ & = 2Mexp(-2\epsilon ^2n)
\end{aligned} \tag{7} \label{7}
$$

由公式$\eqref{7}$的推导可以看出，我们的算法learning得好不好，还与假设空间$\mathcal{H}$里决策函数的个数$M$有关。当$M$有限时，那么数据量$n$越大，发生bad sample的可能性越低。同理如果$M$太大，遇到bad sample的概率也会越大。

综上所述，从概率论的角度出发，根据Hoeffding不等式，可以证明当$E\_{in}(h)$与$E\_{out}(h)$很接近，同时$E\_{in}(h)$与$E\_{out}(h)$都比较小的时候，那么机器学习算法是可learning的。在learning的过程中，如果$E\_{in}(h)$明显的小于$E\_{out}(h)$，那么我们的模型便发生overfiting了；相反，如果$E\_{in}(h)$本身就特别大，那么我们把这种情况称作underfitting。

## Probably Approximately Correct Learnable

为了简便起见，这里以分类场景为例。

假定$f$表示样本的真实“概念”，是从样本空间$X$到类别空间$Y$的映射，它决定了样本$x$的真实标记$y$，如果对于任意的样例$(x, y)$，都有$f(x)=y$成立，则称$f$为目标概念。在训练过程中，由于我们并不确定算法是否能够找到真正的$f$，因此将算法学到的概念称作“假设（hypotheses）”，那么算法考虑的所有可能的“hypotheses”便构成了假设空间（hypotheses space，记作$\mathcal{H}$）。理想的情况下，我们希望最终学习得到的模型$g (g \in \mathcal{H})$可以与$f$等价$(g \equiv  f)$，以0误差完美的做出决策，但在现实中，这种情况很难发生，原因有三：

- 有限的训练样本数决定了训练模型的上限；
- 从分布$D$采样得到的数据具有一定的偶然性，因此不同大小的训练集，得到的结果也可能有所不同；
- 训练数据中可能存在bad sample，导致训练时发生一定的误导性；

因此，为了尽可能达到预设上限的模型，我们需要弱化我们对学习器的要求，不要求学习器能够输出零错误率的假设，只要求错误率以置信度为$\delta$被限制在某个可以接受的常数$\varepsilon$范围内，并且$\varepsilon$越小越好。简而言之，我们只要求学习器可以以较大的概率（至少是1-$\delta$）学习到一个“近似正确（Approximately Correct）”的假设，这就是“Probably Approximately Correct”的由来，简称“PAC”。形象的描述

> **PAC Identify**：给定$0 < \varepsilon,\delta <１$，对于所有的$f \in \mathcal{F}$ 和数据分布$D$，若学习算法的输出空间$g \in \mathcal{H}$满足

>$$P(E(g) \leq \varepsilon) \geq 1 - \delta$$

>则称学习算法能从假设空间$\mathcal{H}$中以较大的概率$(\geq 1 - \delta)$学习到目标概念$f$的近似假设（误差最多为$\varepsilon$），简称PAC Identify。

当有了上述条件，便可以得到PAC Learnable的定义：

> **PAC Learnable**: 令$n$表示从分布$\mathcal{D}$中独立同分布采样得到的样本数，假设空间为$\mathcal{H}$，真实概念类为$\mathcal{F}$，且$0 < \varepsilon,\delta <１$，对于分布$\mathcal{D}$，若存在学习算法$\Psi $和多项式函数$poly(.,.,.,.)$，使得对于任意的$n \geq poly( \frac{1}{\varepsilon}, \frac{1}{\delta}, size(x), size(f))$，算法$\Psi$能够从假设空间$\mathcal{H}$中PAC Identify 真实概念类$\mathcal{F}$，则称概念类$\mathcal{F}$对假设空间$\mathcal{H}$而言是PAC可学习的。

从PAC可学习的定义可以看出，训练样本的数量与学习所需的计算资源密切相关。我们称满足PAC学习算法$\Psi$所需的$n \geq poly( \frac{1}{\varepsilon}, \frac{1}{\delta}, size(x), size(f))$中最小的$n$为学习算法$\mathcal{\Psi}$的样本复杂度。假定学习器对每个训练样本需要某个最小的处理时间，那么如果要使目标函数$g$是PAC可学习的，学习器就必须在多项式数量的训练样本中进行学习。

## Vapnic-Chervonenkis Dimension
前面提到了假设空间$\mathcal{H}$，当假设空间比较多时，此时式$\eqref{6}$就变成了

$$\sum\_{h \in \mathcal{H}} P(\left \| E\_{in}(h) - E\_{out}(h) \right \| \geq \epsilon) \leq 2 \color{red}{ \left \| \mathcal{H} \right \|} exp \left \( -2n \epsilon^2 \right \) \tag{8} \label{8}$$

其中$\left \| \mathcal{H} \right \|$是假设空间的大小。根据式$\eqref{8}$可以看出，当$\left \| \mathcal{H} \right \|$比较大时，经验误差$E\_{in}(h)$和泛化误差$E\_{out}(h)$逼近的概率也就越大，然而与此同时，我们想要从hypotheses space中选择出一条很好的假设就比较困难；相反，如果$\left \| \mathcal{H} \right \|$比较小，经验误差$E\_{in}(h)$和泛化误差$E\_{out}(h)$逼近的概率也就越小了。所以，为了找到一个折衷的衡量经验误差与泛化误差逼近程度的表达式，我们需要对$\left \| \mathcal{H} \right \|$进行转化。因此，VC维便诞生了。

在进入VC维之前，先引入下面几个概念：

- Dichotomy 
- Growth function 
- Shatter && Break point

### Dichotomy

首先来看看dichotomy的定义：

> **Dichotomy**： 给定hypotheses space$\mathcal{H}$和样本集$D = \lbrace x\_1, x\_2, \cdots, x\_n \rbrace $， 假设从$\mathcal{H}$中任意选择一个方程$h$，并让$h$对$D$进行二元分类，最后输出一个结果向量，我们把这个输出向量称为一个Dichotomy。

例如，在$2\mathbb{D}$平面空间，用一条直线对$2$个点进行binary classification，输出可能的结果为$ \lbrace 1,–1  \rbrace $，$ \lbrace –1,1 \rbrace$，$ \lbrace 1,1 \rbrace $，$ \lbrace–1,–1 \rbrace$，每个结果对都是一个dichotomy。对于二分类而言，Dichotomy最多有$2^n$种可能。

虽然hypotheses space 中$\left \| \mathcal{H} \right \|$可以很大，但有效的假设$h$（即dichotomy）是有限的，于是，我们可以用dichotomy的有效数量来取代有限hypotheses space 的Hoeffding不等式中的$\left \| \mathcal{H} \right \|$，即

$$\sum\_{h \in \mathcal{H}} P(\left \| E\_{in}(h) - E\_{out}(h) \right \| \geq \epsilon) \leq 2 \cdot \color{red}{effective(n)} \cdot exp \left \( -2n \epsilon^2 \right \) \tag{9} \label{9}$$

### Growth function

由于dichotomy最多有$2^n$种可能，因此当样本量$n$增加时，dichotomy的可能结果数也会增加。对于所有的$n \in \mathbb{N}$, 假设空间$\mathcal{H}$的growth function $\mathbb{G}\_{\mathcal{H}} (n)$为

$$\mathbb{G}\_{\mathcal{H}} (n) =\underset{\lbrace x\_1, x\_2, \cdots, x\_n \rbrace \subseteq X  }{ max} \left \| \lbrace (h(x\_1), h(x\_2), \cdots, h(x\_n) \| h \epsilon \mathcal{H} \rbrace \right  \| \tag{10} \label{10}$$

对于式子$\eqref{10}$，$\mathbb{G}\_{\mathcal{H}} (n) $的upper bound是$2^n$。更严格一点为下式

$$
\begin{aligned}
\mathbb{G}\_{\mathcal{H}}(n)\leq \sum\_{i=0}^{d\_{vc}}\binom {n}{i}\leq n^{d\_{vc}} ,
\textit{( for }n\geq 2, d\_{vc}\geq 2\textit{ )}
\end{aligned}
\tag{11} \label{11}$$

当Growth function表示假设空间$\mathcal{H}$对$n$个示例所能赋予标记的最大可能的结果数。显然，结果数越多，$\mathcal{H}$的表达能力越强。由此可见，growth function描述的是假设空间$\mathcal{H}$的表达能力，尽管$\mathcal{H}$可能包含无穷多个假设，但其对$D$中示例赋予标记的可能结果数是有限的。

### Shatter && Break point

有了Growth function和Dichotomy的概念之后，下面我们来定义Shatter：

> 当假设空间$\mathcal{H}$作用于$n$个输入样本集时， 产生的dichotomy的数量等于这$n$个点总的组合数$2^n$，也就是Growth function的取值为$2^n$，即$\mathbb{G}\_{\mathcal{H}} (n)  = 2^n$，此时我们就称这$n$个输入被hypotheses space $\mathcal{H}$“Shatter”。

在一些资料中，“shatter”被翻译为“打散”。对于break point这个概念，使用下面这个例子来说明，会更加直观。

假定在某个枪击游戏中，你有一把散弹枪，每个关卡里面都只装有$6$颗子弹，而敌人的数量会随着关卡的不同而不同，具体数量为$2^i$，$i$为关卡号。现在，进入第一关时，敌人的数量为$2^1 = 2$个，很明显你可以shatter掉所有敌人并可以轻松过关；当进入第二关时，敌人数量为$2^2 = 4$，你也可以勉强过关；但是当你进入第三关时，敌人数量为$2^3 = 8$个，而你的子弹数只有$6$颗，此时你就无法通过散弹枪顺利过关了。我们把这个关卡$3$就叫做**break point**。

### VC dimension

了解了growth function、dichotomy、shatter、break point这些概念之后，这时，我们也可以使用growth function来估计$E\_{in}$和$E\_{out}$的关系，式$\eqref{9}$变为下式：

$$\sum\_{h \in \mathcal{H}} P(\left \| E\_{in}(h) - E\_{out}(h) \right \| \geq \epsilon) \leq \color{red}{4 \cdot \mathbb{G}\_{\mathcal{H}} (2n) } \cdot exp \left \( - \color{red}{\frac{1}{8}}n \epsilon^2 \right \) \tag{12} \label{12}$$

式$\eqref{11}$就是我们的**VC bound**。这个公式的意义在于：如果假设空间$\mathcal{H}$存在有限的break point $k$，那么$\mathbb{G}\_{\mathcal{H}} (2n) $会被最高幂次为$k–1$的多项式上界给约束住。随着$n$的逐渐增大，指数式$exp(*)$的下降会比多项式$\mathbb{G}$的增长速度更快，所以此时可以推断出VC Bound是有界的。更进一步，当$n$足够大时，对于$\mathcal{H}$中的任意一个假设$h$，$E\_{in}(h)$都将接近于$E\_{out}(h)$，表示学习是可行的。

现在，我们现在可以定义VC维了，此概念由Vladimir Vapnik与Alexey Chervonenkis提出。

> **VC Dimension**：给定假设空间$\mathcal{H}$，它的VC维是能被$\mathcal{H}$ shatter的最大实例集大小$n$，
>$$VC(\mathcal{H}) = max \lbrace n: \mathcal{G}\_{\mathcal{H}}(n) = 2^n \rbrace $$

简而言之，若存在大小为$n$的示例集能被假设空间$\mathcal{H}$打散，但不存在任何大小为$n+1$的示例集能被$\mathcal{H}$打散，则假设空间$\mathcal{H}$的VC维是$n$。此时，break point为$n+1$。若对于任意数目的样本都有$2^n$个函数能将它们打散，则函数集的VC维是无穷大。

由于寻找所有hypothesis space的growth function是困难的，因此我们使用$n^{d\_{vc}}$作为hypothesis space中所有$VC(\mathcal{H})=d\_{vc}$的growth function的上界。So，对于hypothesis space $\mathcal{H}$的任意一个$g$来说，都有：

$$
\begin{aligned}\;\;\;\,
\mathbb{P}[|E\_{in}(g) - E\_{out}(g) \ge \epsilon|] 
&\leq \mathbb{P}[BAD]  \\\
&= \mathbb{P}[\exists h \in \mathcal{H}\text{ s.t. } |E\_{in}(h)-E\_{out}(h)|\ge \epsilon] \\\
&\leq 4\mathbb{G}\_{\mathcal{H}}(2n) exp \left \( - \frac{1}{8}  n\epsilon^2 \right \) \\\
&\leq 4(2n)^{d\_{vc}}exp \left \(- \frac{1}{8} n\epsilon^2 \right \) , (\textit{ if }d\_{vc}\textit{ is finite })
\end{aligned}
\tag{13} \label{13}$$

因此，如果想要让机器学到东西，并且学习得到的结果比较好，就得满足三个条件：

1. Good $\mathcal{H}$：假设空间$\mathcal{H}$的VC维$d\_{vc}$是有限的;
2. Good $D$：适中的样本量$n$，这样才能确保vc bound的不会太大；
3. Good $\Psi$：一个好的算法，确保能够从假设空间$\mathcal{H}$中挑选出一个能使$E\_{in}$最小的函数$g$。

VC维反映了函数集的学习能力，VC维越大，学习机器越复杂。根据前面的推导，我们知道VC维的大小：与学习算法$\Psi$无关，与输入变量$X$的分布也无关，与我们求解的目标函数$g$无关，只与模型和假设空间$\mathcal{H}$有关。实践中有这样一个规律：假设空间 $\mathcal{H}$的VC 维与假设参数$w$的自由变量数目大约相等。即$VC(\mathcal{H}) = free \ parameters$。

最后，令之前得到的VC Bound为$\delta$，则坏事情发生的概率$P[|E\_{in}(g)−E\_{out}(g)|>\epsilon] \le \delta$，好事情发生的概率$P[|E\_{in}(g)−E\_{out}(g)|≤\epsilon] \ge 1−\delta$，从而可以推导出$\epsilon$：

$$
\begin{aligned}
&P[|E\_{in}(g)-E\_{out}(g)|\ge \epsilon] \le \delta \\\
\Rightarrow  
&P[|E\_{in}(g)-E\_{out}(g)|\leq  \epsilon] \ge 1 - \delta \\\
&\text{set}\;\;\;\; 
\delta = 4(2n)^{d\_{vc}}exp(-\frac{1}{8}n\epsilon^2)\\\
\Rightarrow  
& \epsilon = \sqrt{\frac{8}{n}ln(\frac{4(2n)^{d\_{vc}}}{\delta})}
\end{aligned}
\tag{14} \label{14}$$

根据$\eqref{14}$的结果，进而可以推导出$E\_{in}$与$E\_{out}$的关系：

$$
E\_{in}(g)-\sqrt{\frac{8}{n}ln(\frac{4(2n)^{d\_{vc}}}{\delta})} \leq E\_{out}(g) \leq E\_{in}(g)+ \color{red}{ \underset{\Omega }{\underbrace{\sqrt{\frac{8}{n}ln(\frac{4(2n)^{d\_{vc}}}{\delta})}}}} 
\tag{15} \label{15}$$


当然，我们比较关心的还是$E\_{out}$的上界，式$\eqref{15}$红色部分为模型的复杂度$\Omega$，模型越复杂，$E\_{in}$与$E\_{out}$的逼近程度越远。随着VC维的上升，$E\_{in}$会不断降低，而$\Omega$项会不断上升，其上升与下降的速度在每个阶段都有所不同，因此我们能够寻找一个二者兼顾的，比较合适的$d\_{vc}$，用来决定应该使用多复杂的模型。反过来，如果我们需要使用$d\_{vc}=3$这种复杂度的模型，并且想保证$\epsilon=0.1$，置信度$1−\delta=0.9$，我们也可以通过VC Bound来求得大致需要的数据量$n$。通过简单的计算可以得到，理论上我们需要$n \approx 10,000d\_{vc}$大小的数据量，但VC Bound事实上是一个极为宽松的bound，因为它对于任何演算法$\Psi$，任何分布的数据$D$，以及任何目标函数$g$都成立，所以经验上，常常认为$n \approx 10d\_{vc}$就可以有不错的结果。

另外，还可以从VC维的角度解释正则项的作用。在训练的时候，我们是从假设空间$\mathcal{H}$中寻找最佳的$g$时，然而为了避免overfiting，通常会加上正则项。当正则项加入之后，新的hypotheses space会加入一些新的bound，这时新假设空间的VC维也将变小。换言之，对于同样的训练数据，$E\_{in}$更有可能等于$E\_{out}$，从而泛化能力变得更强。

## Conclusion

虽然本文大致的叙述了VC维，但因笔者自身水平有限，很多东西仅仅只是一隅之见，理解的并不到位，如果有措辞上的不妥，还请读者告之。此外，VC维可以说是机器学习中的一大核心，并非一篇文章就可以讲解透彻，本文相对于VC维而言，仅仅是冰山一角，如果想深入理解的，还请前去查阅相关paper。

## References

- Hoeffding, W. (1963). Probability Inequalities for Sums of Bounded Random Variables. Journal of the American Statistical Association, 58(301), 13–30.
- Kearns, M. J., & Vazirani, U. V. (1994). An introduction to computational learning theory. Cambridge, MA, USA: MIT Press.
- 周志华《Machine Learning 机器学习》第12章 计算学习理论.
- [Wikipedia- Vapnik–Chervonenkis theory](https://en.wikipedia.org/wiki/VC_dimension)
- [VC Theory: Hoeffding Inequality](http://freemind.pluskid.org/slt/vc-theory-hoeffding-inequality/)
- [VC Theory: Symmetrization](http://freemind.pluskid.org/slt/vc-theory-symmetrization)
- [VC Theory: Vapnik–Chervonenkis Dimension](http://freemind.pluskid.org/slt/vc-theory-vapnik-chervonenkis-dimension/)
- [林轩田老师《机器学习基石》课程笔记](http://beader.me/mlnotebook/)
- [Flickering - VC维的来龙去脉](http://www.flickering.cn/machine_learning/2015/04/vc%E7%BB%B4%E7%9A%84%E6%9D%A5%E9%BE%99%E5%8E%BB%E8%84%89/)
