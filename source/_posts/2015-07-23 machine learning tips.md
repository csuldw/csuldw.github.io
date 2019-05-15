---
layout: post
date: 2015-07-23 12:53
title: "scikit-klean交叉验证"
tags: 
	- Machine Learning
	- 交叉验证
	- Cross-Validation
comment: true
categories: ML
---

__一个Windows操作系统能够使用的pythonIDE__
> winPython下载地址：[WinPython_2.7](http://sourceforge.net/projects/winpython/files/WinPython_2.7/2.7.10.1/)


传统的F-measure或平衡的F-score (F1 score)是精度和召回的调和平均值：

$$F_1 = 2 \times \dfrac{precision \times recall}{precision + recall}$$

<!-- more -->

### __1.Cross Validation （交叉验证）__

cross validation大概的意思是：对于原始数据我们要将其一部分分为train_data，一部分分为test_data。train_data用于训练，test_data用于测试准确率。在test_data上测试的结果叫做validation_error。将一个算法作用于一个原始数据，我们不可能只做出随机的划分一次train和test_data，然后得到一个validation_error，就作为衡量这个算法好坏的标准。因为这样存在偶然性。我们必须好多次的随机的划分train_data和test_data，分别在其上面算出各自的validation_error。这样就有一组validation_error，根据这一组validation_error，就可以较好的准确的衡量算法的好坏。

cross validation是在数据量有限的情况下的非常好的一个evaluate performance的方法。而对原始数据划分出train data和test data的方法有很多种，这也就造成了cross validation的方法有很多种。

sklearn中的cross validation模块，最主要的函数是如下函数：
sklearn.cross_validation.cross_val_score:他的调用形式是scores = cross_validation.cross_val_score(clf, raw_data, raw_target, cv=5, score_func=None)

__参数解释：__

__clf__:表示的是不同的分类器，可以是任何的分类器。比如支持向量机分类器。clf = svm.SVC(kernel='linear', C=1)；   
__raw_data__：原始数据；  
__raw_target__:原始类别标号；  
__cv__：代表的就是不同的cross validation的方法了。引用scikit-learn上的一句话（When the cv argument is an integer, cross_val_score uses the KFold or StratifiedKFold strategies by default, the latter being used if the estimator derives from ClassifierMixin.）如果cv是一个int数字的话，那么默认使用的是KFold或者StratifiedKFold交叉，如果如果指定了类别标签则使用的是StratifiedKFold。  
__cross_val_score__:这个函数的返回值就是对于每次不同的的划分raw_data时，在test_data上得到的分类的**准确率**。至于准确率的算法可以通过score_func参数指定，如果不指定的话，是用clf默认自带的准确率算法。  

scikit-learn的cross-validation交叉验证代码：

```
>>> from sklearn import cross_validation
>>> from sklearn import svm
>>> clf = svm.SVC(kernel='linear', C=1)
>>> scores = cross_validation.cross_val_score(clf, iris.data, iris.target, cv=5)#5-fold cv
# change metrics
>>> from sklearn import metrics
>>> cross_validation.cross_val_score(clf, iris.data, iris.target, cv=5, score_func=metrics.f1_score)
#f1 score: http://en.wikipedia.org/wiki/F1_score
```
  
Note: if using LR, clf = LogisticRegression().

__生成一个数据集做为交叉验证__

```
>>> import numpy as np
>>> from sklearn.cross_validation import train_test_split
>>> X, y = np.arange(10).reshape((5, 2)), range(5)
>>> X
array([[0, 1],
       [2, 3],
       [4, 5],
       [6, 7],
       [8, 9]])
>>> list(y)
[0, 1, 2, 3, 4]
```

__将数据切分为训练集和测试集__

```
>>> X_train, X_test, y_train, y_test = train_test_split(
...     X, y, test_size=0.33, random_state=42)
...
>>> X_train
array([[4, 5],
       [0, 1],
       [6, 7]])
>>> y_train
[2, 0, 3]
>>> X_test
array([[2, 3],
       [8, 9]])
>>> y_test
[1, 4]
```

__交叉验证的使用__

下面是手动划分训练集和测试集，控制台中输入下列代码进行测试：

```
>>> import numpy as np
>>> from sklearn import cross_validation
>>> from sklearn import datasets
>>> from sklearn import svm
>>> iris = datasets.load_iris()
>>> iris.data.shape, iris.target.shape
((150, 4), (150,))
>>> X_train, X_test, y_train, y_test = cross_validation.train_test_split(
...     iris.data, iris.target, test_size=0.4, random_state=0)
>>> X_train.shape, y_train.shape
((90, 4), (90,))
>>> X_test.shape, y_test.shape
((60, 4), (60,))
>>> clf = svm.SVC(kernel='linear', C=1).fit(X_train, y_train)
>>> clf.score(X_test, y_test)                           
0.96...
```

下面是交叉验证的实例：

```
>>> clf = svm.SVC(kernel='linear', C=1)
>>> scores = cross_validation.cross_val_score(
...    clf, iris.data, iris.target, cv=5)
...
>>> scores                                              
array([ 0.96...,  1.  ...,  0.96...,  0.96...,  1.        ])
```

通过cross_validation，设置cv=5，进行5倍交叉验证，最后得到一个scores的预测准确率数组，表示每次交叉验证得到的准确率。

```
>>> print("Accuracy: %0.2f (+/- %0.2f)" % (scores.mean(), scores.std() * 2))
Accuracy: 0.98 (+/- 0.03)
```

通过scores.mean()求出平均值，得到平均精度。还可以通过指定scoring来设置准确率算法

```
>>> from sklearn import metrics
>>> scores = cross_validation.cross_val_score(clf, iris.data, iris.target,
...     cv=5, scoring='f1_weighted')
>>> scores                                              
array([ 0.96...,  1.  ...,  0.96...,  0.96...,  1.        ])
```

__libsvm格式的数据导入：__


```
>>> from sklearn.datasets import load_svmlight_file
>>> X_train, y_train = load_svmlight_file("/path/to/train_dataset.txt")
...
>>>X_train.todense()#将稀疏矩阵转化为完整特征矩阵
```

------

### __2.处理非均衡问题__

对于正负样本比例相差较大的非均衡问题，一种调节分类器的方法就是对分类器的训练数据进行改造。一种是**欠抽样**，一种是**过抽样**。过抽样意味着赋值样例，而欠抽样意味着删除样例。对于过抽样，最后可能导致过拟合问题；而对于欠抽样，则删掉的样本中可能包含某些重要的信息，会导致欠拟合。对于正例样本较少的情况下，通常采取的方式是**使用反例类别的欠抽样和正例类别的过抽样相混合的方法**




---

### __3.scikit-learn学习SVM__

```
>>> from sklearn import datasets
>>> iris = datasets.load_iris()
>>> digits = datasets.load_digits()
>>> print digits.data
[[  0.   0.   5. ...,   0.   0.   0.]
 [  0.   0.   0. ...,  10.   0.   0.]
 [  0.   0.   0. ...,  16.   9.   0.]
 ..., 
 [  0.   0.   1. ...,   6.   0.   0.]
 [  0.   0.   2. ...,  12.   0.   0.]
 [  0.   0.  10. ...,  12.   1.   0.]]
>>> digits.target
array([0, 1, 2, ..., 8, 9, 8])
>>> digits.images[0]
array([[  0.,   0.,   5.,  13.,   9.,   1.,   0.,   0.],
       [  0.,   0.,  13.,  15.,  10.,  15.,   5.,   0.],
       [  0.,   3.,  15.,   2.,   0.,  11.,   8.,   0.],
       [  0.,   4.,  12.,   0.,   0.,   8.,   8.,   0.],
       [  0.,   5.,   8.,   0.,   0.,   9.,   8.,   0.],
       [  0.,   4.,  11.,   0.,   1.,  12.,   7.,   0.],
       [  0.,   2.,  14.,   5.,  10.,  12.,   0.,   0.],
       [  0.,   0.,   6.,  13.,  10.,   0.,   0.,   0.]])
>>> from sklearn import svm
>>> clf = svm.SVC(gamma=0.001, C=100.)
>>> clf.fit(digits.data[:-1],digits.target[:-1])
SVC(C=100.0, cache_size=200, class_weight=None, coef0=0.0, degree=3,
  gamma=0.001, kernel='rbf', max_iter=-1, probability=False,
  random_state=None, shrinking=True, tol=0.001, verbose=False)
>>> clf.predict(digits.data[-1])
array([8])
>>> 
```

---


### __4.scikit-learn学习RandomForest__


使用例子

```
>>> from sklearn.ensemble import RandomForestClassifier
>>> X = [[0, 0], [1, 1]]
>>> Y = [0, 1]
>>> clf = RandomForestClassifier(n_estimators=10)
>>> clf = clf.fit(X, Y)
```

__Method__


![](/assets/articleImg/2015-07-21 randomForest分类器的方法png.png)

randomForestClassifier分类器的初始值

```
def __init__(self,
	 n_estimators=10,
	 criterion="gini",
	 max_depth=None,
	 min_samples_split=2,
	 min_samples_leaf=1,
	 min_weight_fraction_leaf=0.,
	 max_features="auto",
	 max_leaf_nodes=None,
	 bootstrap=True,
	 oob_score=False,
	 n_jobs=1,
	 random_state=None,
	 verbose=0,
	 warm_start=False,
	 class_weight=None):
```

------

<br>

