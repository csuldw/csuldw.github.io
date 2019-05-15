---
layout: post
date: 2019-01-20 02:31
title: "推荐系统"
categories: ML
tag:
	- 推荐系统
	- 深度学习
  	- Embedding
  	- Keras
comment: true
---

### Introduction

上篇文章中提到，推荐系统的两个主要阶段：召回和排序。这里再补充一个阶段，在技术实现上推荐系统一般可划分3个阶段：

1. 挖掘：挖掘user信息和item信息；
2. 召回：根据user对item的兴趣进行召回；
3. 排序：对召回的item进行排序，推荐top K。

<!-- more -->

本文主要介绍信息流推荐中的下面几类推荐：

1. Item popularity Recommendation
2. Content-Based Recommendation
3. Tag-Based Recommendation
4. User-Based CF Recommendation
5. Item-Based CF Recommendation
6. Item Embedding Recommendation
7. Model-Based Recommendation

### Item popularity Recommendation

根据流行度进行推荐，主要是根据物品的点击行为进行统计，比如本周点击量最多的Top k个item、本月Topk、本季度TopK、etc。这种推荐策略非常简单，而且易于计算，缺点是没法捕捉用户的兴趣，ctr往往偏低。

### Content-Based Recommendation

首先找到用户的兴趣，然后根据用户兴趣从doc中推荐相似的doc。

### Tag-Based Recommendation

基于标签进行召回推荐。

### User-Based CF Recommendation

基于用户的协同过滤

### Item-Based CF Recommendation

基于Item的协同过滤，通过user-item的行为关系，找出具有相同行为的item，然后对用户按照item进行推荐。

### Item Embedding Recommendation

基于item embedding进行推荐。

### Model-Based Recommendation

基于模型的推荐算法，包括SVD、FM、FFM、DeepFM、etc

#### Factorization Machine

Factorization Machine,简称FM，是由Steffen Rendle提出的一种基于矩阵分解的ML算法，主要用于解决数据稀疏的业务场景（如推荐业务、CTR预估）特征如何组合的问题。

FM与SVM相比，优势如下：

1. FM可以实现非常稀疏数据参数估计，而SVM会效果不理想，因为训出的SVM模型会面临较高的bias；
FMs拥有线性的复杂度, 可以通过 primal 来优化而不依赖于像SVM的支持向量机；
2. 词的序列其实等价于一系列连续操作的item序列，因此，训练语料只需将句子改为连续操作的item序列即可，item间的共现为正样本，并按照item的频率分布进行负样本采样。

### References

1. https://blog.csdn.net/qq_23269761/article/details/81366939#commentBox