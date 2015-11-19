---
layout: post
date: 2015-10-25 10:24
title: "scikit-learn Preprocessing"
categories: ML
tag: 
	- preprocessing
	- Machine Learning
comment: true
---

本文主要是对照[scikit-learn的preprocessing](http://scikit-learn.org/stable/modules/preprocessing.html#preprocessing)章节结合代码简单的回顾下预处理技术的几种方法，主要包括标准化、数据最大最小缩放处理、正则化、特征二值化和数据缺失值处理。内容比较简单，仅供参考！

首先来回顾一下下面要用到的基本知识。

<!-- more -->
## **一、知识回顾**

均值公式：

$$\bar{x}=\frac{1}{n}\sum\_{i=1}^{n}x_{i}$$

方差公式：

$$s^{2}=\frac{1}{n}\sum\_{i=1}^{n}(x_{i}-\bar{x})^{2}$$

0-范数，向量中非零元素的个数。

1-范数：

$$||X||= \sum\_{i=1}^{n} |x_{i}|$$

2-范数：

$$||X||\_{2} =  (\sum\_{i=1}^{n} x_{i}^{2})^{\frac{1}{2}}$$

p-范数的计算公式：

$$||X||_{p}=(|x1|^{p}+|x2|^{p}+...+|xn|^{p})^{\frac{1}{p}}$$

---

数据标准化：当单个特征的样本取值相差甚大或明显不遵从高斯正态分布时，标准化表现的效果较差。实际操作中，经常忽略特征数据的分布形状，移除每个特征均值，划分离散特征的标准差，从而等级化，进而实现数据中心化。

## **二、标准化(Standardization)，或者去除均值和方差进行缩放**

公式为：(X-X_mean)/X_std 计算时对每个属性/每列分别进行.

将数据按其属性(按列进行)减去其均值，然后除以其方差。最后得到的结果是，对每个属性/每列来说所有数据都聚集在0附近，方差值为1。

首先说明下sklearn中preprocessing库里面的scale函数使用方法：

```
sklearn.preprocessing.scale(X, axis=0, with_mean=True,with_std=True,copy=True)
```

根据参数的不同，可以沿任意轴标准化数据集。

参数解释：

- X：数组或者矩阵
- axis：int类型，初始值为0，axis用来计算均值 means 和标准方差 standard deviations. 如果是0，则单独的标准化每个特征（列），如果是1，则标准化每个观测样本（行）。
- with_mean: boolean类型，默认为True，表示将数据均值规范到0
- with_std: boolean类型，默认为True，表示将数据方差规范到1

**一个简单的例子**

假设现在我构造一个数据集X，然后想要将其标准化。下面使用不同的方法来标准化X：

**方法一：使用sklearn.preprocessing.scale()函数**

**方法说明：**

- X.mean(axis=0)用来计算数据X每个特征的均值；
- X.std(axis=0)用来计算数据X每个特征的方差；
- preprocessing.scale(X)直接标准化数据X。

将代码整理到一个文件中：

```
from sklearn import preprocessing 
import numpy as np
X = np.array([[ 1., -1.,  2.],
              [ 2.,  0.,  0.],
              [ 0.,  1., -1.]])
# calculate mean
X_mean = X.mean(axis=0)
# calculate variance 
X_std = X.std(axis=0)
# standardize X
X1 = (X-X_mean)/X_std
# use function preprocessing.scale to standardize X
X_scale = preprocessing.scale(X)
```

最后X_scale的值和X1的值是一样的，前面是单独的使用数学公式来计算，主要是为了形成一个对比，能够更好的理解scale()方法。

**方法2：sklearn.preprocessing.StandardScaler类**

该方法也可以对数据X进行标准化处理，实例如下：

```
from sklearn import preprocessing 
import numpy as np
X = np.array([[ 1., -1.,  2.],
              [ 2.,  0.,  0.],
              [ 0.,  1., -1.]])
scaler = preprocessing.StandardScaler()
X_scaled = scaler.fit_transform(X)
```

这两个方法得到最后的结果都是一样的。

---

## **三、将特征的取值缩小到一个范围（如0到1）**

除了上述介绍的方法之外，另一种常用的方法是将属性缩放到一个指定的最大值和最小值(通常是1-0)之间，这可以通过preprocessing.MinMaxScaler类来实现。

使用这种方法的目的包括：

- 1、对于方差非常小的属性可以增强其稳定性；
- 2、维持稀疏矩阵中为0的条目。

下面将数据缩至0-1之间，采用MinMaxScaler函数

```
from sklearn import preprocessing 
import numpy as np
X = np.array([[ 1., -1.,  2.],
              [ 2.,  0.,  0.],
              [ 0.,  1., -1.]])
min_max_scaler = preprocessing.MinMaxScaler()
X_minMax = min_max_scaler.fit_transform(X)
```
最后输出：

```
array([[ 0.5       ,  0.        ,  1.        ],
       [ 1.        ,  0.5       ,  0.33333333],
       [ 0.        ,  1.        ,  0.        ]])
```

测试用例：

```
>>> X_test = np.array([[ -3., -1.,  4.]])
>>> X_test_minmax = min_max_scaler.transform(X_test)
>>> X_test_minmax
array([[-1.5       ,  0.        ,  1.66666667]])
```

注意：这些变换都是对列进行处理。



当然，在构造类对象的时候也可以直接指定最大最小值的范围：feature_range=(min, max)，此时应用的公式变为：

```
X_std=(X-X.min(axis=0))/(X.max(axis=0)-X.min(axis=0))
X_minmax=X_std/(X.max(axis=0)-X.min(axis=0))+X.min(axis=0))
```
---

## **四、正则化(Normalization)**

正则化的过程是将每个样本缩放到单位范数(每个样本的范数为1)，如果要使用如二次型(点积)或者其它核方法计算两个样本之间的相似性这个方法会很有用。

该方法是文本分类和聚类分析中经常使用的向量空间模型（Vector Space Model)的基础.

Normalization主要思想是对每个样本计算其p-范数，然后对该样本中每个元素除以该范数，这样处理的结果是使得每个处理后样本的p-范数(l1-norm,l2-norm)等于1。

**方法1：使用sklearn.preprocessing.normalize()函数**

```
>>> X = [[ 1., -1.,  2.],
...      [ 2.,  0.,  0.],
...      [ 0.,  1., -1.]]
>>> X_normalized = preprocessing.normalize(X, norm='l2')
>>> X_normalized                                      
array([[ 0.40..., -0.40...,  0.81...],
       [ 1.  ...,  0.  ...,  0.  ...],
       [ 0.  ...,  0.70..., -0.70...]])
```

**方法2：sklearn.preprocessing.StandardScaler类**

```
>>> normalizer = preprocessing.Normalizer().fit(X)  # fit does nothing
>>> normalizer
Normalizer(copy=True, norm='l2')
```

然后使用正则化实例来转换样本向量：

```
>>> normalizer.transform(X)                            
array([[ 0.40..., -0.40...,  0.81...],
       [ 1.  ...,  0.  ...,  0.  ...],
       [ 0.  ...,  0.70..., -0.70...]])
>>> normalizer.transform([[-1.,  1., 0.]])             
array([[-0.70...,  0.70...,  0.  ...]])
```

两种方法都可以，效果是一样的。


---

## **五、二值化(Binarization)**

特征的二值化主要是为了将数据特征转变成boolean变量。在sklearn中，sklearn.preprocessing.Binarizer函数可以实现这一功能。实例如下：

```
>>> X = [[ 1., -1.,  2.],
...      [ 2.,  0.,  0.],
...      [ 0.,  1., -1.]]
>>> binarizer = preprocessing.Binarizer().fit(X)  # fit does nothing
>>> binarizer
Binarizer(copy=True, threshold=0.0)
>>> binarizer.transform(X)
array([[ 1.,  0.,  1.],
       [ 1.,  0.,  0.],
       [ 0.,  1.,  0.]])
```

Binarizer函数也可以设定一个阈值，结果数据值大于阈值的为1，小于阈值的为0，实例代码如下：

```
>>> binarizer = preprocessing.Binarizer(threshold=1.1)
>>> binarizer.transform(X)
array([[ 0.,  0.,  1.],
       [ 1.,  0.,  0.],
       [ 0.,  0.,  0.]])
```

---

## **六、缺失值处理**

由于不同的原因，许多现实中的数据集都包含有缺失值，要么是空白的，要么使用NaNs或者其它的符号替代。这些数据无法直接使用scikit-learn分类器直接训练，所以需要进行处理。幸运地是，sklearn中的**Imputer**类提供了一些基本的方法来处理缺失值，如使用均值、中位值或者缺失值所在列中频繁出现的值来替换。

下面是使用均值来处理的实例：

```
>>> import numpy as np
>>> from sklearn.preprocessing import Imputer
>>> imp = Imputer(missing_values='NaN', strategy='mean', axis=0)
>>> imp.fit([[1, 2], [np.nan, 3], [7, 6]])
Imputer(axis=0, copy=True, missing_values='NaN', strategy='mean', verbose=0)
>>> X = [[np.nan, 2], [6, np.nan], [7, 6]]
>>> print(imp.transform(X))                           
[[ 4.          2.        ]
 [ 6.          3.666...]
 [ 7.          6.        ]]
```

Imputer类同样支持稀疏矩阵：

```
>>> import scipy.sparse as sp
>>> X = sp.csc_matrix([[1, 2], [0, 3], [7, 6]])
>>> imp = Imputer(missing_values=0, strategy='mean', axis=0)
>>> imp.fit(X)
Imputer(axis=0, copy=True, missing_values=0, strategy='mean', verbose=0)
>>> X_test = sp.csc_matrix([[0, 2], [6, 0], [7, 6]])
>>> print(imp.transform(X_test))                      
[[ 4.          2.        ]
 [ 6.          3.666...]
 [ 7.          6.        ]]
```


本文讲解的比较接单，如果对这些不是很理解的话，请到scikit-learn的官网中查看英文版本：[preprocessing](http://scikit-learn.org/stable/modules/preprocessing.html#preprocessing).

## **References**

- [Scikit-learn preprocessing](http://scikit-learn.org/stable/modules/preprocessing.html#preprocessing).



---






