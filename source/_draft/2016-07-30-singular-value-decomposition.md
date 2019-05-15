---
layout: post
date: 2016-07-30 16:24
title: "Singular Value Decomposition"
categories: ML
tag:
	- Machine Learning
	- SVD
comment: true
---

- 对称矩阵特征值对应的特征向量相互正交。

TF/IDF

根据关键字k1,k2,k3进行搜索结果的相关性就变成TF1\*IDF1 + TF2\*IDF2 + TF3\*IDF3。比如document1的term总量为1000，k1,k2,k3在document1出现的次数是100，200，50。包含了k1, k2, k3的document总量分别是 1000，10000，5000。document set的总量为10000。 TF1 = 100/1000 = 0.1 TF2 = 200/1000 = 0.2 TF3 = 50/1000 = 0.05 IDF1 = In（10000/1000） = In (10) = 2.3 IDF2 = In（10000/10000） = In (1) = 0; IDF3 = In（10000/5000） = In (2) = 0.69 这样关键字k1,k2,k3与document1的相关性= 0.1\*2.3 + 0.2\*0 + 0.05\*0.69 = 0.2645 其中k1比k3的比重在document1要大，k2的比重是0.


三个矩阵有非常清楚的物理含义。第一个矩阵X中的每一行表示意思相关的一类词，其中的每个非零元素表示这类词中每个词的重要性（或者说相关性），数值越大越相关。最后一个矩阵Y中的每一列表示同一主题一类文章，其中每个元素表示这类文章中每篇文章的相关性。中间的矩阵则表示类词和文章雷之间的相关性。因此，我们只要对关联矩阵A进行一次奇异值分解，w 我们就可以同时完成了近义词分类和文章的分类。（同时得到每类文章和每类词的相关性）。

## 参考文献

- http://www.ams.org/samplings/feature-column/fcarc-svd
- 译文http://blog.csdn.net/redline2005/article/details/24100293
- http://www.cise.ufl.edu/~srajaman/Rajamanickam_S.pdf