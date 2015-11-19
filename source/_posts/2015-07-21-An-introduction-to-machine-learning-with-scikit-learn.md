---
layout: post
title: "机器学习scikit-learn入门教程（译）"
date: 2015-07-21 21:31
comment: true
tags: 
	- Machine Learning
categories: ML
---

原文链接：http://scikit-learn.github.io/dev/tutorial/basic/tutorial.html

__章节内容__

在这个章节中，我们主要介绍关于scikit-learn机器学习词库，并且将给出一个学习样例。

## **机器学习：问题设置**

通常，一个学习问题是通过一系列的n个样本数据来学习然后尝试预测未知数据的属性。如果每一个样本超过一个单一的数值，例如多维输入（也叫做多维数据），那么它就拥有了多个特征。
<!-- more -->
我们可以把学习问题划分为几个大的来别：

* 监督学习: 在监督学习中，这些数据自带了我们想要预测的附加属性（[scikit-learn监督学习链接](http://scikit-learn.github.io/dev/supervised_learning.html#supervised-learning)），这个问题包括：
	* 分类：样本属于属于两类或者多类，我们想从已经被标记的数据中来预测未知数据的类别。一个分类问题的例子就是手写字识别。这个例子的目的是从有些的类别中识别出输入向量的类别。对于分类的另一种想法是作为监督学习的一种分离的表格(不是连续的)，在这个表格中，一个是被限制的类别数量，而且对于每个类别都有N个样例被提供；一个是尝试用正确的类别或者类来标记他们。
	* 回归：如果期望的输出是由一个或者更多的连续的变量组成，那么就叫做回归。回归问题的例子将通过一条鲑鱼的年龄和重量预测它的长度。
* 无监督学习：在无监督学习里面，训练数据是由一组没有任何类别标签值的一系列输入向量组成。这种问题的目的是可能可以在这些数据里发现相似的样例组，这些相似的样例被称作聚类。或者在输入空间里决定数据分布，称之为密度估算；或者将数据从高维空间映射到二维或三维空间中，称之为数据可视化问题。（[无监督学习链接](http://scikit-learn.github.io/dev/unsupervised_learning.html#unsupervised-learning)）

**训练集和测试集**

机器学习是关于学习数据集的一些属性然后将它们应用到新的数据上。这就是为什么在机器学习中评价一个算法的通常惯例是把数据集切分为两个数据集，其中一个叫做训练集，用来学习数据的属性；另一个叫做测试集，在测试集上测试那些属性。


## **加载样本数据集**


scikit-learn带有一些标准的数据集，例如用于分类的[iris](http://en.wikipedia.org/wiki/Iris_flower_data_set)和[digit](http://archive.ics.uci.edu/ml/datasets/Pen-Based+Recognition+of+Handwritten+Digits)数据集和用于回归的[ boston house prices dataset ](http://archive.ics.uci.edu/ml/datasets/Housing).

下面，我们打开Python编译器，然后载入__iris__和digits数据集。我们的符号'$'表示shell提示，'>>>'表示Python编译器提示

```
$ python
>>> from sklearn import datasets
>>> iris = datasets.load_iris()
>>> digits = datasets.load_digits()
```

 数据集是一个类似字典的对象，包含所有的数据和一些和数据有关的元数据。数据存储在.data中，是个n_samples,n_features的数组。在监督问题的情况下，一个或多个类别变量存储在.target成员中。更多有关的不同数据集的细节可以在[dedicated section](http://scikit-learn.github.io/dev/datasets/index.html#datasets)查找。
 
 例如，在digits数据集情况下，digits.data 提供了可用于分类数字样本。

```
>>> print(digits.data)  
[[  0.   0.   5. ...,   0.   0.   0.]
 [  0.   0.   0. ...,  10.   0.   0.]
 [  0.   0.   0. ...,  16.   9.   0.]
 ...,
 [  0.   0.   1. ...,   6.   0.   0.]
 [  0.   0.   2. ...,  12.   0.   0.]
 [  0.   0.  10. ...,  12.   1.   0.]]
```

并且digits.target给出了digit数据集的真实结果，这些数字是和我们正在学习的每个数字图像相关的数字。

```
>>> digits.target
array([0, 1, 2, ..., 8, 9, 8])
```

**数组的形状**

数据总是一些2D数组，shape(n_samples,n_features),尽管原始数据也许有一个不同的形状，就这个digits而言，每一个原始样例是一个shape(8,8)的图像，并且能被访问使用:

```
>>> digits.images[0]
array([[  0.,   0.,   5.,  13.,   9.,   1.,   0.,   0.],
       [  0.,   0.,  13.,  15.,  10.,  15.,   5.,   0.],
       [  0.,   3.,  15.,   2.,   0.,  11.,   8.,   0.],
       [  0.,   4.,  12.,   0.,   0.,   8.,   8.,   0.],
       [  0.,   5.,   8.,   0.,   0.,   9.,   8.,   0.],
       [  0.,   4.,  11.,   0.,   1.,  12.,   7.,   0.],
       [  0.,   2.,  14.,   5.,  10.,  12.,   0.,   0.],
       [  0.,   0.,   6.,  13.,  10.,   0.,   0.,   0.]])
```

[simple example on this dataset ](http://scikit-learn.github.io/dev/auto_examples/classification/plot_digits_classification.html#example-classification-plot-digits-classification-py)这个数据集表明了在scikit-learn中怎样从原始问题开始着手制作数据。



## **学习和预测**

在digits数据集中，给定一幅手写数字的数字图像，任务是预测结果。我们给定的样本有10种类别（是数字0到9），基于此我们建立一个估计方法能够预测我们没有见过的样本属于哪一类。

在scikit-learn中，用于分类的估计模型是一个实现了fit(x,y)方法和predict(T)方法的Python对象。
    
估计模型的例子是在实现了[support vector classification支持向量机](http://en.wikipedia.org/wiki/Support_vector_machine)的类 sklearn.svm.SVC。估计模型的构造函数带有模型参数，但是目前，我们将估计模型当做一个黑盒子。

```
>>> from sklearn import svm  
>>> clf = svm.SVC(gamma=0.001, C=100.)
```

**选择模型参数**
	
在这个例子中，我们这设定了gamma值。可以通过使用[网格搜索](http://scikit-learn.github.io/dev/modules/grid_search.html#grid-search)和[交叉验证](http://scikit-learn.github.io/dev/modules/cross_validation.html#cross-validation)自动的找出最好的参数值

我们把我们的评估模型命名为clf，作为一个分类器，它现在必须拟合这个模型，也就是它必须从这个模型学习。我们通过将数据集传递给fit函数完成。作为训练集，除了最后一个样本，我们选择其余的所有样本。通过python语句[:-1]选择样本，这条语句将从digits.data中产生一个除了最后一个样本的新数组。

```
clf.fit(digits.data[:-1], digits.target[:-1])    
SVC(C=100.0, cache_size=200, class_weight=None, coef0=0.0, degree=3,  
  gamma=0.001, kernel='rbf', max_iter=-1, probability=False,  
  random_state=None, shrinking=True, tol=0.001, verbose=False)  
```

现在，我们可以预测新值，尤其是我们可以问分类器在digits数据集中的用来训练分类器时没有使用的最后一个数据是数字几：

相应的图像如下所示:

<center>
![这里写图片描述](http://img.blog.csdn.net/20150720185355481)
</center>

正如你看到的，这是一个具有挑战性的任务：图象的分辨率很低。你认同这个分类器吗？

一个完整的分类问题实例可以通过下面的链接下载，用来作为你运行并且学习的例子 [Recognizing hand-written digits](http://scikit-learn.github.io/dev/auto_examples/classification/plot_digits_classification.html#example-classification-plot-digits-classification-py)

## **模型持久化**

可以通过使用python的built-in持久化模型在scikit中保存一个模型，命名[pickle](http://docs.python.org/library/pickle.html):

```
>>> from sklearn import svm
>>> from sklearn import datasets
>>> clf = svm.SVC()
>>> iris = datasets.load_iris()
>>> X, y = iris.data, iris.target
>>> clf.fit(X, y)  
SVC(C=1.0, cache_size=200, class_weight=None, coef0=0.0,
  decision_function_shape=None, degree=3, gamma='auto', kernel='rbf',
  max_iter=-1, probability=False, random_state=None, shrinking=True,
  tol=0.001, verbose=False)
>>> import pickle
>>> s = pickle.dumps(clf)
>>> clf2 = pickle.loads(s)
>>> clf2.predict(X[0])
array([0])
>>> y[0]
0
```

在scikit的特别情况下，使用joblib替换pickle(joblib.dump & joblib.load)会更有趣,它在大数据上是更有效的，但是仅仅只能存入的是字典而不是字符串。

```
>>> from sklearn.externals import joblib
>>> joblib.dump(clf, 'filename.pkl') 
```

然后你就可以读取上面的pickled模型使用了（通常是在其它的Python程序中）：

```
>>> clf = joblib.load('filename.pkl') 
```

## **惯例**

scikit-learn估计量有一些特定的规则是的分类器更具有预测性

**Type casting 类型转换**

除非特别指定，否则输入格式是float64

```
>>> import numpy as np
>>> from sklearn import random_projection
>>> rng = np.random.RandomState(0)
>>> X = rng.rand(10, 2000)
>>> X = np.array(X, dtype='float32')
>>> X.dtype
dtype('float32')
>>> transformer = random_projection.GaussianRandomProjection()
>>> X_new = transformer.fit_transform(X)
>>> X_new.dtype
dtype('float64')
```

在这个例子中，X是float32，通过fit_transform(X)把它转为float64

回归的输出值是float64，分类的也是：

```
>>> from sklearn import datasets
>>> from sklearn.svm import SVC
>>> iris = datasets.load_iris()
>>> clf = SVC()
>>> clf.fit(iris.data, iris.target)  
SVC(C=1.0, cache_size=200, class_weight=None, coef0=0.0,
  decision_function_shape=None, degree=3, gamma='auto', kernel='rbf',
  max_iter=-1, probability=False, random_state=None, shrinking=True,
  tol=0.001, verbose=False)
>>> list(clf.predict(iris.data[:3]))
[0, 0, 0]
>>> clf.fit(iris.data, iris.target_names[iris.target])  
SVC(C=1.0, cache_size=200, class_weight=None, coef0=0.0,
  decision_function_shape=None, degree=3, gamma='auto', kernel='rbf',
  max_iter=-1, probability=False, random_state=None, shrinking=True,
  tol=0.001, verbose=False)
>>> list(clf.predict(iris.data[:3]))  
['setosa', 'setosa', 'setosa']
```

这里，第一次predict()返回的是一个整数数组，因为在拟合中用到了iris.target（一个整数数组），第二个predict返回的是一个字符串数组，因为用来拟合的是iris.target_names。


## Supplementary

推介一个好用的python IDE：
> winPython下载地址：[WinPython_2.7](http://sourceforge.net/projects/winpython/files/WinPython_2.7/2.7.10.1/)

---

<br>


