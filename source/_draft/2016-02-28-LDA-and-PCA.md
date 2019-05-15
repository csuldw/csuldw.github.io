---
layout: post
date: 2016-02-26 20:24
title: "机器学习算法选择"
categories: ML
tag: 
	- ML
	- 算法选择
    - 偏差
	- 方差
	- LR
comment: true
---

LDA（linear discriminat analysis）线性判别分析

- [Wikipedia - Linear discriminant analysis](https://en.wikipedia.org/wiki/Linear_discriminant_analysis)
- [机器学习中的数学(4)-线性判别分析（LDA）, 主成分分析(PCA)](http://www.cnblogs.com/LeftNotEasy/archive/2011/01/08/lda-and-pca-machine-learning.html)
- [JerryLead之LDA线性判别分析](http://www.cnblogs.com/jerrylead/archive/2011/04/21/2024384.html)


PCA（principle component analysis） 主成分分析

- [JerryLead之PCA主成分分析](http://www.cnblogs.com/jerrylead/archive/2011/04/18/2020209.html)

FA (factor analysis）因子分析

LDA思想：最大类间距离，最小类内距离。简而言之：第一，为了实现投影后的两个类别的距离较远，用映射后两个类别的均值差的绝对值来度量。第二，为了实现投影后，每个类内部数据点比较聚集，用投影后每个类别的方差来度量。

PCA思想: 将n维特征映射到k维上（k<n），这k维是全新的正交特征。这k维特征称为主元，是重新构造出来的k维特征，而不是简单地从n维特征中去除其余n-k维特征。

<font color="#007fff">**一般来说，方差大的方向是信号的方向，方差小的方向是噪声的方向，我们在数据挖掘中或者数字信号处理中，往往要提高信号与噪声的比例，也就是信噪比。**</font>对上图来说，如果我们只保留signal方向的数据，也可以对原数据进行不错的近似了。

以下内容引自 [Wikipedia- Linear discriminant analysis](https://en.wikipedia.org/wiki/Linear_discriminant_analysis)
><font color="green">**LDA is also closely related to principal component analysis (PCA) and factor 		analysis in that they both look for linear combinations of variables which best explain the data. LDA explicitly attempts to model the difference between the classes of data. PCA on the other hand does not take into account any difference in class, and factor analysis builds the feature combinations based on differences rather than similarities. Discriminant analysis is also different from factor analysis in that it is not an interdependence technique: a distinction between independent variables and dependent variables (also called criterion variables) must be made.**</FONT>


区别：<font color="#007fff">**PCA选择样本点投影具有最大方差的方向，LDA选择分类性能最好的方向。**</font>


LDA的一些限制：

- LDA至多可生成C-1维（C表示类别数）子空间，即LDA降维后的维度区间在[1,C-1]，与原始特征数n无关，对于二值分类，最多投影到1维。
- LDA不适合对非高斯分布样本进行降维。
- LDA在样本分类信息依赖方差而不是均值时，效果不好。
- LDA可能过度拟合数据。

SVD