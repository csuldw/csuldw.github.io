

## 预处理One-Hot Encoding

One-Hot Encoding的目的是将特征数字化，并且经过One-Hot编码后，原来特征的m个取值，就会变成m个二元特征，并且这些特征互斥，每次只有一个激活，因此数据就编程稀疏的。

好处：

- 解决分类器不好处理属性特征的问题
- 在一定程度上扩充了特征

##实例

我们基于python和Scikit-learn写一个简单的例子：

```
from sklearn import preprocessing
en = preprocessing.OneHotEncoder()
m1 = [[0,0,3],[1,1,0],[0,2,1],[1,0,2]]
em.fit(m1)
em.transform([1,0,2]).toarray()
```
输出结果：

~~~
$| array([[ 0.,  1.,  1.,  0.,  0.,  0.,  0.,  1.,  0.]])
~~~

