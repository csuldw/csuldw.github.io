


关于优化理论，这里推荐一本书籍（有时间去拜读一下）：[Convex Optimization](http://stanford.edu/~boyd/cvxbook/)

本文主要讲解的是无约束优化方法。



 二次型函数：  给定n阶实对称矩阵，对于任意的非零向量X，如果有$X^TQX>0$,则称Q是正定矩阵。

对称矩阵Q为正定的充要条件是：<font color="#007FFF">**Q的特征值全为正。**</font>

二次函数$f(x)=1/2X^TQX + b^TX+c$，若Q是正定的，则称f(x)为正定二次函数。





优化方法有多种：

- 梯度下降法
- 坐标下降法
- 牛顿法
- 拟牛顿法-DFP
- [拟牛顿法-BFGS](http://blog.csdn.net/acdreamers/article/details/44664941)
- [L-BFGS](http://blog.csdn.net/henryczj/article/details/41542049?utm_source=tuicool&utm_medium=referral)
- 共轭方向法
- 共轭梯度法
- EM


首先介绍下泰勒公式：

![$$f(x) = \frac{f(x)}{0!} + \frac{f'(x_0)}{1!}(x-x_0) + \frac{f''(x_0)}{2!}(x-x_0)^2 + \cdots + \frac{f^{(n)}(x_0)}{n!}(x-x_0)^n + R(x_0)$$](http://latex.codecogs.com/png.latex?%5Cinline%20%5Cdpi%7B150%7D%20%5Cbg_white%20f%28x%29%20%3D%20%5Cfrac%7Bf%28x%29%7D%7B0%21%7D%20&plus;%20%5Cfrac%7Bf%27%28x_0%29%7D%7B1%21%7D%28x-x_0%29%20&plus;%20%5Cfrac%7Bf%27%27%28x_0%29%7D%7B2%21%7D%28x-x_0%29%5E2%20&plus;%20%5Ccdots%20&plus;%20%5Cfrac%7Bf%5E%7B%28n%29%7D%28x_0%29%7D%7Bn%21%7D%28x-x_0%29%5En%20&plus;%20R%28x_0%29)

牛顿法求得是Hessian矩阵，而拟牛顿法是用Hessian矩阵的逆矩阵代替Hessian矩阵。



## 参考文献

[1] [Machine Learning and Optimization](http://freemind.pluskid.org/series/mlopt/)    
[2] [Gradient Descent, Wolfe's Condition and Logistic Regression](http://freemind.pluskid.org/machine-learning/gradient-descent-wolfe-s-condition-and-logistic-regression)     
[3] [Newton Method](http://freemind.pluskid.org/machine-learning/newton-method/)  
[4] [机器学习优化算法—L-BFGS](http://blog.csdn.net/henryczj/article/details/41542049?utm_source=tuicool&utm_medium=referral)  
[5] [BFGS算法](http://blog.csdn.net/acdreamers/article/details/44664941)  
[6] [华夏35度-无约束优化方法](http://www.cnblogs.com/zhangchaoyang/articles/2600491.html)