---
layout: post
date: 2016-03-19 15:04
title: "随机森林和mRMR特征选择"
categories: ML
tag:
	- Machine Learning
	- mRMR
	- Feature Selection
	- Random Forest
comment: true
---



算法性能的好坏跟数据是密不可分的，因此找到一组更具代表性的特征子集显得更加重要。在实际项目中，因为有的特征对模型而言是冗余的，它对算法的性能会产生负面影响，此时就需要做特征选择。特征选择的目的就是从一组特征集合中去除冗余或不相关的特征从而达到降维的目的。说到降维，它不仅包括特征选择，还包括了特征提取，而本文主要介绍两种常用的特征选择方法。

<!-- more -->

对于一个包含n个特征的特征集合，搜索空间高达$2^n - 1$种可能的子集，所以如果是使用穷举法，不得不说下，穷举法的结果确实很好，特征少的时候或许可以考虑，但特征多的时候必将带来不可估量的计算量。所以我们需要找一种相对折衷的方法。也就是下面谈到的RF和mRMR。

## Random Forest

<font color="#007ff">**注意：随机森林使用的CART算法的方法增长树，也就是使用Gini指数来划分**</font>。Gini指数度量的是数据分区或训练集D的不纯度（注意，这里是不纯度，跟熵有点不同）。基尼不纯度表示的是一个随机选中的样本在子集中被分错的可能性。基尼不纯度为这个样本被选中的概率乘上它被分错的概率。当一个节点中所有样本都是一个类时，基尼不纯度为零。 定义为：

![$$Gini(D) = 1 - \sum_{i=1}^m p_i^2$$](http://latex.codecogs.com/gif.latex?%24%24Gini%28D%29%20%3D%201%20-%20%5Csum_%7Bi%3D1%7D%5Em%20p_i%5E2%24%24)

假设A有v个不同的值出现在特征D中，它的二元划分有$2^v - 2$种（除去自己和空集）。当考虑二元划分裂时，计算每个结果分区的不纯度加权和。比如A有两个值，则特征D被划分成D1和D2,这时Gini指数为：

![$$Gini_A(D) = \frac{D_1}{D} Gini(D_1) + \frac{D_2}{D} Gini(D_2)$$](http://latex.codecogs.com/gif.latex?Gini_A%28D%29%20%3D%20%5Cfrac%7BD_1%7D%7BD%7D%20Gini%28D_1%29%20&plus;%20%5Cfrac%7BD_2%7D%7BD%7D%20Gini%28D_2%29)

Gini指数偏向于多值属性，并且当类的数量很大时会有困难，而且它还倾向于导致相等大小的分区和纯度。但实践效果不错。

如果训练数据集有m维,样本个数为n,则 CART算法的时间复杂度为$ Ο (m n logn^2)$ 。

互信息是条件概率与后验概率的比值，化简之后就可以得到信息增益。所以说互信息其实就是信息增益。计算方法【互信息=熵-条件熵】。熵描述的是不确定性。熵越大，不确定性就越大，条件熵H（B|A）描述的是在A给定的条件下B的不确定性，如果条件熵越小，表示不确定性就越大，那么B就越容易确定结果。所以使用熵减去条件熵，就得到了信息增益，他描述的不确定性的降低程度，可以用来度量两个变量的相关性。比如，在给定一个变量的条件下，另一个变量它的不确定性能够降低多少，如果不确定性降低得越多，那么它的确定性就越大，就越容易区分，两者就越相关。

$$IG(D, A) = H(D) - H(D|A)$$


随机森林对于每一棵决策树，首先对列（特征）进行采样，然后计算当前的Gini指数，随后进行全分裂过程，每棵树的非叶节点都有一个Gini指数，一棵树建立之后可以得到该树各个节点的重要性，通过对其按照Gini指数作为特征相关性来排序，接着一次建立多棵决策树，并且生成多个特征相关性排名，最后对这些特征选平均值，得到最终排好序的特征重要性排名。


随机森林OOB特征选择：

- 首先建立m棵决策树，然后分别计算每棵树的OOB袋外误差errOOBj。
- 计算特征$x_i $的重要性。随机的修改OOB中的每个特征$x_i $的值，再次计算它的袋外误差errOOBi；$x_i 的重要性=\sum\frac{errOOBi-errOOBj}{Ntree}$.
- 按照特征的重要性排序，然后剔除后面不重要的特征；
- 然后重复以上步骤，直到选出m个特征。


在scikit-learn中封装了random forest特征选择方法：

```
from sklearn.datasets import load_boston
from sklearn.ensemble import RandomForestRegressor
import numpy as np
#Load boston housing dataset as an example
boston = load_boston()
X = boston["data"]
Y = boston["target"]
names = boston["feature_names"]
rf = RandomForestRegressor()
rf.fit(X, Y)
print "Features sorted by their score:"
print sorted(zip(map(lambda x: round(x, 4), rf.feature_importances_), names), 
             reverse=True)

```

最后输出的是：

>Features sorted by their score:
[(0.5298, 'LSTAT'), (0.4116, 'RM'), (0.0252, 'DIS'), (0.0172, 'CRIM'), (0.0065, 'NOX'), (0.0035, 'PTRATIO'), (0.0021, 'TAX'), (0.0017, 'AGE'), (0.0012, 'B'), (0.0008, 'INDUS'), (0.0004, 'RAD'), (0.0001, 'CHAS'), (0.0, 'ZN')]

## mRMR

最大相关最小冗余（mRMR），顾名思义，我们可以知道，它不仅考虑到了特征和label之间的相关性，还考虑到了特征和特征之间的相关性。度量标准使用的是互信息(Mutual information)。对于mRMR方法，<font color="#007FFF">**特征子集与类别的相关性通过各个特征与类别的信息增益的均值来计算，而特征与特征的冗余使用的是特征和特征之间的互信息加和再除以子集中特征个数的平方**</font>，因为I(xi,xj)计算了两次。

**No.1 最大相关性**

目的：保证特征和类别的相关性最大。

![$$max \ D(S, c),\  D = \frac{1}{|S|} \sum_{x_i \epsilon S } I(x_i; c)$$](http://latex.codecogs.com/gif.latex?max%20%5C%20D%28S%2C%20c%29%2C%5C%20D%20%3D%20%5Cfrac%7B1%7D%7B%7CS%7C%7D%20%5Csum_%7Bx_i%20%5Cepsilon%20S%20%7D%20I%28x_i%3B%20c%29)
 

**No.2 最小冗余性**

目的：确保特征之间的冗余性最小。

![min\ R(S, c),\ \ R = \frac{1}{|S|^2} \sum_{x_i,x_j \epsilon S } I(x_i; x_j)](http://latex.codecogs.com/gif.latex?min%5C%20R%28S%2C%20c%29%2C%5C%20%5C%20R%20%3D%20%5Cfrac%7B1%7D%7B%7CS%7C%5E2%7D%20%5Csum_%7Bx_i%2Cx_j%20%5Cepsilon%20S%20%7D%20I%28x_i%3B%20x_j%29)

两个式子中，S表示已经选择的特征子集，c表示classs\_label，x表示特征。最后选择标准是：

$$max \  \Phi(D,R) , \Phi = D - R$$

得到的子集在保证特征与类别的相关性较大的同时，还保证了特征的冗余性最小。


关于mRMR实现，可以到github上面去下载：[网址链接](https://github.com/csuldw/MachineLearning/tree/master/mRMR)




## 小结

使用RF和mRMR进行特征选择后，都会得到一个重要性排名。接下来通常需要结合交叉验证来选择结果性能最好的特征子集。比较原始的方法就是，根据排名对特征子集从`top1-topn`一个个进行交叉验证测试，然后选择结果最好的一组特征即可。


## 文献

- 《数据挖掘概念与技术》，韩家炜；
- [Minimum Redundancy and Maximum Relevance Feature selection](https://www.google.co.jp/url?sa=t&rct=j&q=&esrc=s&source=web&cd=2&cad=rja&uact=8&ved=0ahUKEwitzvCwpMzLAhUFUaYKHQi8A5IQFggmMAE&url=http%3A%2F%2Fpenglab.janelia.org%2Fproj%2FmRMR%2FBIBM07_mRMR_071103_handout.pdf&usg=AFQjCNFh9Rqy1qlJjqUABlFuaY4yvBsPTA&sig2=YxQvuBTk64GkAaZ560gznQ)
- [Variable selection using Random Forests ](https://www.google.co.jp/url?sa=t&rct=j&q=&esrc=s&source=web&cd=5&cad=rja&uact=8&sqi=2&ved=0ahUKEwiWho6tpczLAhWEppQKHdr0BdEQFgg7MAQ&url=https%3A%2F%2Fhal.archives-ouvertes.fr%2Fhal-00755489%2Ffile%2FPRLv4.pdf&usg=AFQjCNGq5RLaeyLLQXzBbsKvL_UUn-mflw&sig2=XuJTB29C5kU1f0WAtJwwfg)
- [Selecting good features – Part III: random forests](http://blog.datadive.net/selecting-good-features-part-iii-random-forests/)
- [mRMR (minimum Redundancy Maximum Relevance Feature Selection)](http://penglab.janelia.org/proj/mRMR/)