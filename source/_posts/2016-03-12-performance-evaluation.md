---
layout: post
date: 2016-03-12 20:03
title: "分类之性能评估指标"
categories: ML
tags:
	- Machine Learning
	- ROC
	- AUC
---

出处：http://www.csuldw.com/2016/03/12/2016-03-12-performance-evaluation/  

本文主要介绍几种常用的分类评估指标，同时介绍如何绘制ROC曲线以及AUC值的便捷的计算方法。最后再附上一个绘制ROC曲线和计算AUC的源码实现。

<!-- more -->

## Precision和Recall

首先我们来看看下面这个混淆矩阵：

|pred\_label/true\_label|Positive|Negative|  
|--------|-----|----|  
|Positive|TP|FP|  
|Negtive|FN|TN|    

如上表所示，行表示预测的label值，列表示真实label值。TP，FP，FN，TN分别表示如下意思：

- TP（true positive）：表示样本的真实类别为正，最后预测得到的结果也为正；
- FP（false positive）：表示样本的真实类别为负，最后预测得到的结果却为正；
- FN（false negative）：表示样本的真实类别为正，最后预测得到的结果却为负；
- TN（true negative）：表示样本的真实类别为负，最后预测得到的结果也为负.


根据以上几个指标，可以分别计算出Accuracy、Precision、Recall（Sensitivity，SN），Specificity（SP）。

$$Accuracy = \frac{TP+TN}{TP+FP+TN+FN}$$

$$Precision = \frac{TP}{TP+FP}$$

$$Recall = \frac{TP}{TP+FN}$$

$$SP = \frac{TN}{TN + FP}$$

- Accuracy：表示预测结果的精确度，预测正确的样本数除以总样本数。
- precision，准确率，表示预测结果中，预测为正样本的样本中，正确预测为正样本的概率；
- recall，召回率，表示在原始样本的正样本中，最后被正确预测为正样本的概率；
- specificity，常常称作特异性，它研究的样本集是原始样本中的负样本，表示的是在这些负样本中最后被正确预测为负样本的概率。

在实际当中，我们往往希望得到的precision和recall都比较高，比如当FN和FP等于0的时候，他们的值都等于1。但是，它们往往在某种情况下是互斥的，比如这种情况，50个正样本，50个负样本，结果全部预测为正，那么它的precision为1而recall却为0.5.所以需要一种折衷的方式，因此就有了F1-score。

$$ F1-score = \frac{ 2 \times recall \times precision}{ recall + precision}$$

F1-score表示的是precision和recall的调和平均评估指标。


此外还有MCC：

![$$MCC = \frac{TP \times TN - FP \times FN}{ \sqrt {(TP + FP)(TP + FN)( TN + FP)(TN+FN)}}$$](http://latex.codecogs.com/gif.latex?%24%24MCC%20%3D%20%5Cfrac%7BTP%20%5Ctimes%20TN%20-%20FP%20%5Ctimes%20FN%7D%7B%20%5Csqrt%20%7B%28TP%20&plus;%20FP%29%28TP%20&plus;%20FN%29%28%20TN%20&plus;%20FP%29%28TN&plus;FN%29%7D%7D%24%24)



## ROC曲线

ROC（receiver operating characteristic），平面的横坐标是false positive rate(FPR)假阳率，纵坐标是true positive rate(TPR)真阳率。ROC计算过程如下：

- 首先每个样本都需要有一个label值，并且还需要一个预测的score值（取值0到1）;
- 然后按这个score对样本由大到小进行排序，假设这些数据位于表格中的一列，从上到下依次降序;
- 现在从上到下按照样本点的取值进行划分，位于分界点上面的我们把它归为预测为正样本，位于分界点下面的归为负样本;
- 分别计算出此时的TPR（Recall）=TP/P和FPR（1-SP）=FP/N，然后在图中绘制（FPR, TPR）点。

从上往下逐个样本计算，最后会得到一条光滑的曲线 。

然而，千言万语不如下面的这幅图懂得快：

![](http://www.csuldw.com/assets/articleImg/roc_plot.gif)

<div class="caption">『roc曲线绘制动画—图片来自[参考文献5](http://stats.stackexchange.com/questions/105501/understanding-roc-curve/105577).』</div>


## AUC计算

AUC（area under the curve）就是ROC曲线下方的面积，取值在0.5到1之间，因为随机猜测得到额AUC就是0.5。面积如下图所示，阴影部分即为AUC面积：

![](http://www.csuldw.com/assets/articleImg/area_under_curve.png)

<div class="caption">『AUC面积图解—图片来自[参考文献5](http://stats.stackexchange.com/questions/105501/understanding-roc-curve/105577).』</div>

AUC的几种解释（来自[【Interpreting the AUROC】](http://stats.stackexchange.com/questions/132777/what-does-auc-stand-for-and-what-is-it?)）:

- The expectation that a uniformly drawn random positive is ranked before a uniformly drawn random negative.
- The expected proportion of positives ranked before a uniformly drawn random negative.
- The expected true positive rate if the ranking is split just before a uniformly drawn random negative.
- The expected proportion of negatives ranked after a uniformly drawn random positive.
- The expected false positive rate if the ranking is split just after a uniformly drawn random positive.


下面来介绍下它的计算方法，AUC的计算主要有以下三种。

第一种：积分思维。这也是在早期机器学习文献中常用的AUC计算方法。从积分的思想中演化而来的。假如我们的测试样本有限，那么我们得到的AUC曲线必然是呈现阶梯形状。因此，计算的AUC也就是这些阶梯下面的面积之和（有没有想起以前学高数时的积分面积哈）。我们可以这样来计算，首先把score值进行排序，假设score越大，此样本属于正类的概率就越大。然后一边扫描一边计算就可以得到我们想要的AUC。但是，这样做会有个缺点，当多个测试样本的score值相等时，我们调整一下阈值，得到的不是往上或者往右的延展，而是斜着向上形成一个梯形。此时，就需要计算这个梯形的面积，这样是比较麻烦。 简单的用代码描述下

```
auc = 0.0
height = 0.0

for each training example x_i, y_i：
  if y_i = 1.0:
    height = height + tpr
  else 
    auc +=  height * fpr

return auc
```


第二种：[Mann–Whitney U test（MWW）](https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U_test)。关于AUC还有一个很有趣的性质，它和Wilcoxon-Mann-Witney Test类似（可以去google搜一下），而Wilcoxon-Mann-Witney Test就是<font color="#007FFF">**测试任意给一个正类样本和一个负类样本，正类样本的score有多大的概率大于负类样本的score**</font>。有了这个定义，就可以得到了另外一中计算AUC的方法：计算出这个概率值。我们知道，在有限样本中我们常用的得到概率的办法就是通过频率来估计之。这种估计随着样本规模的扩大而逐渐逼近真实值。样本数越多，计算的AUC越准确类似，也和计算积分的时候，小区间划分的越细，计算的越准确是同样的道理。具体来说就是：<font color="red"> 统计一下所有的 M×N(M为正类样本的数目，N为负类样本的数目)个正负样本对中，有多少个组中的正样本的score大于负样本的score。当二元组中正负样本的 score相等的时候，按照0.5计算。然后除以MN。实现这个方法的复杂度为O(n^2  )。n为样本数(即n=M+N)</font>,公式表示如下：


![$$**AUC = \frac{\sum_i^n ( \ pos\_score  > neg\_score \ )}{M * N}**$$](http://latex.codecogs.com/gif.latex?AUC%20%3D%20%5Cfrac%7B%5Csum_i%5En%20%28%20%5C%20pos%5C_score%20%3E%20neg%5C_score%20%5C%20%29%20&plus;%200.5%20*%20%5Csum_i%5En%7B%28%5C%20pos%5C_score%3Dneg%5C_score%5C%20%29%7D%7D%7BM%20*%20N%7D)

第三种：该方法和上述第二种方法原理一样，但复杂度降低了。首先对score从大到小排序，然后令最大score对应的sample的rank值为n，第二大score对应sample的rank值为n-1，以此类推从n到1。然后把所有的正类样本的rank相加，再减去正类样本的score为最小的那M个值的情况。得到的结果就是有多少对正类样本的score值大于负类样本的score值，最后再除以M×N即可。值得注意的是，当存在score相等的时候，对于score相等的样本，需要赋予相同的rank值(无论这个相等的score是出现在同类样本还是不同类的样本之间，都需要这样处理)。具体操作就是再把所有这些score相等的样本 的rank取平均。然后再使用上述公式。此公式描述如下： 

![$$AUC = \frac{\sum_{ins_i \epsilon pos}rank_{ins_i} - \frac{M * (M+1)}{2}}{M * N}$$](http://latex.codecogs.com/gif.latex?AUC%20%3D%20%5Cfrac%7B%5Csum_%7Bins_i%20%5Cepsilon%20pos%7Drank_%7Bins_i%7D%20-%20%5Cfrac%7BM%20*%20%28M&plus;1%29%7D%7B2%7D%7D%7BM%20*%20N%7D)

这三种方法，第一种比较好理解，后面两种确实不太好理解，先记下，慢慢理解。

## 源码

最后，附上ROC曲线绘制代码。下面使用的思想类似积分，但是求得是AUC的近似值，忽略了梯形部分，Code如下：

依赖库：

- numpy
- matplotlib


```
# -*- coding: utf-8 -*-
"""
Created on Sat Mar 12 17:43:48 2016

@author: liudiwei
"""
import numpy as np
import matplotlib.pyplot as plt

def plotROC(predScore, labels):
    point = (1.0, 1.0) #由于下面排序的索引是从小到大，所以这里从(1,1)开始绘制
    ySum = 0.0 
    numPos = np.sum(np.array(labels)==1.0)
    numNeg = len(labels)-numPos
    yStep = 1/np.float(numPos)
    xStep = 1/np.float(numNeg)
    sortedIndex = predScore.argsort() #对predScore进行排序，的到排序索引值
    fig = plt.figure()
    fig.clf()
    ax = plt.subplot(111)
    for index in sortedIndex.tolist()[0]:
        if labels[index] == 1.0: #如果正样本各入加1，则x不走动，y往下走动一步
            delX = 0
            delY = yStep;
        else:                   #否则，x往左走动一步，y不走动
            delX = xStep
            delY = 0
            ySum += point[1]     #统计y走动的所有步数的和
        ax.plot([point[0], point[0] - delX], [point[1], point[1] - delY],c='b')
        point = (point[0] - delX, point[1] - delY)
    ax.plot([0,1],[0,1],'b--')
    plt.xlabel('False positive rate'); plt.ylabel('True positive rate')
    plt.title('ROC Curve')
    ax.axis([0, 1, 0, 1])
    plt.show() 
    #最后，所有将所有矩形的高度进行累加，最后乘以xStep得到的总面积，即为AUC值
    print "the Area Under the Curve is: ", ySum * xStep
``` 

对于ROC曲线绘制中的参数，输入的第二个参数是类别标签（如，+1，-1形成的文件，每行表示一个样本的真实类别）；第一个参数则是由模型训练出来的预测强度，如Adaboost对样本i预测的结果为0.67，对i+1个样本预测的结果是0.3，等等，每行一个，格式和classLabels一样。最后绘制ROC曲线的同时，也在输出ROC曲线下方的AUC面积。


## 参考文献

[1] https://en.wikipedia.org/wiki/Receiver_operating_characteristic  
[2] http://blog.csdn.net/chjjunking/article/details/5933105  
[3]《Machine Learning in Action》
[4] https://en.wikipedia.org/wiki/Mann%E2%80%93Whitney_U_test
[5] [Understanding ROC curve](http://stats.stackexchange.com/questions/105501/understanding-roc-curve/105577)  
[6] http://stats.stackexchange.com/questions/145566/how-to-calculate-area-under-the-curve-auc-or-the-c-statistic-by-hand
