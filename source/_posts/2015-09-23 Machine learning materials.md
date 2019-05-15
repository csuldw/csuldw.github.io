---
layout: post
title: 机器学习-个人资料整理
date: 2015-09-23 22:22:22
tag:
	- Machine Learning
comments: true
categories: ML
---


学习Machine Learning也有很长一段时间了，前段时间在paper中应用了GTB（Gradient Tree Boosting）算法。在我的数据集上GTB的performance比Random Forest要稍微强一点，整个experiment做完之后，有许多东西都来不及及时整理，很多都遗忘了。打算接下来的时间里，好好整理下自己的学习资料，这份资料绝对不是一时半会就整理得完的，先开个头吧，以后会间断性更新该blog的。

下面来做个资料整理吧。

<!-- more -->

## **书籍推荐**

机器学习的书籍很多，下面推荐几本本人用过而且觉得还不错的书籍。优于机器学习是一门跨领域的学科，所以在书籍上并非全是机器学习的书籍:

- 1.《机器学习实战》**Machine Learning in Action [美] Peter Harington 著**。该书贯穿了10个最受欢迎的机器学习算法，提供了案例研究问题并用Python代码实例来解决。我本人比较喜欢这本书，因为里面的代码给了我很大的帮助，自己在学习机器学习算法的时候，理论上很多东西不太理解透，通过该书实践之后，在算法层面又有了进一步的提高。
- 2.《统计学习方法》 李航著。该书比较详细地介绍了算法的原理，只从理论层面来研究算法。通过这本书和《机器学习实战》两本书相结合，一本讲理论，一本着手实践，加在一起会有事半功倍的效果。
- 3.《数据挖掘概念与技术》 韩家炜著。该书介绍了数据挖掘的常用技术，比较详实，但本人觉得不太适合初学者，当时自己初学的时候看的就是这本书，结果最后很多地方理解的不是很好，后来通过《统计学习方法》和算法实践之后，再回头看《数据挖掘概念与技术》，感觉就轻松多了。
- 4.《数学之美》 吴军著。本书可以当做业余书籍来看，可以在无聊的时候看看，不过里面讲的东西还是挺有用的。
- 5.《Python科学计算》该书可以当做Python编程参考书籍，但前提是你喜欢使用Python，并爱上了它，不然这本书还是蛮贵的，我自己也是通过“研究生自由探索项目”才买的这本书，因为可以报销嘛。

## **学习工具**

机器学习的tools很多，这里只列出几个参考工具。

- [Scikit-learn](http://scikit-learn.org/stable/user_guide.html).基于Python语言的[scikit-learn](http://scikit-learn.org/stable/user_guide.html)库，里面涵盖了分类、聚类、回归的大部分算法，并且有常用的评估指标以及预处理数据的方法，是一个不错的学习库，强力推荐。附一篇博文：[SOME USEFUL MACHINE LEARNING LIBRARIES](http://www.erogol.com/broad-view-machine-learning-libraries/).
- [R](http://www.r-project.org/)语言，语言就是一门工具，R语言现在在商业界是用的最多的，在统计方面功能强大，而且也有封装好的算法库可以直接使用。附：[R语言参考卡片](https://cran.r-project.org/doc/contrib/Liu-R-refcard.pdf).
- [Weka](http://www.cs.waikato.ac.nz/ml/weka/)，是一个基于java开发的数据挖掘工具，可以尝试一下。它为用户提供了一系列据挖掘API、命令行和图形化用户接口。你可以准备数据、可视化、建立分类、进行回归分析、建立聚类模型，同时可以通过第三方插件执行其他算法。除了WEKA之外， [Mahout](http://mahout.apache.org/)是Hadoop中为机器学习提供的一个很好的JAVA框架，你可以自行学习。如果你是机器学习和大数据学习的新手，那么坚持学习WEKA，并且全心全意地学习一个库。
- Matlab，里面有很多的工具包，不过本人不怎么用过。参考：[Matlab Codes and Datasets for Feature Learning](http://www.cad.zju.edu.cn/home/dengcai/Data/data.html)和[Statistics and Machine Learning Toolbox](http://cn.mathworks.com/products/statistics/)。此外matlab中的[Octave](http://www.gnu.org/software/octave/)可以很方便地解决线性和非线性问题，比如机器学习算法底层涉及的问题。如果你有工程背景，那么你可以由此入手。
- [BigML](https://bigml.com/):可能你并不想进行编程工作。你完全可以不通过代码，来使用 WEKA那样的工具。你通过使用BigMLS的服务来进行更加深入的工作。BigML通过Web页面，提供了机器学习的接口，因此你可以通过浏览器来建立模型。
- 如果你使用Python，这里推荐一个IDE，[WinPython](http://sourceforge.net/projects/winpython/files/WinPython_2.7/2.7.10.1/),IDE版本就是Python的版本，自行选择！


下面给出一个比较图，具体想要学什么，还需自己抉择。

<center>
![这里写图片描述](http://img.blog.csdn.net/20150918075645450)
</center>


## **学习视频**

由于本人比较崇拜Andrew Ng，所以关于视频，首先推荐的便是Andrew Ng的斯坦福大学的机器学习课程。这套视频在网上有两个网址，国外和国内的都有，全程英语教学，内容很好，有时间建议你去听听：

- 一个是国外的Coursera公开课，该课程在机器学习领域很火，是很多入门学者的首选。地址：https://www.coursera.org/；讲义地址：[Stanford CS229 course下载讲义和笔记](http://cs229.stanford.edu/)；
- 一个是国内的网易公开课，链接地址：http://open.163.com/movie/2008/1/U/O/M6SGF6VB4_M6SGJURUO.html

下面是一个机器学习视频库，由加州理工学院（Caltech）出品。

- 机器学习视频库，地址：http://work.caltech.edu/library/

其它的视频库

- [Machine Learning Category on VideoLectures](http://videolectures.net/Top/Computer_Science/Machine_Learning/)，这个网站的视频比较多。你可以找出比较感兴趣的资源，然后深入学习。

<font color="#008B00">机器学习最近在国内比较火，许多培训机构都相应的开了该门课程，如果想要听中文教程的，可以去网上搜索下，这里就不给培训机构打广告了。</font>

## **博客和文章推荐**

大牛们的博客，会让你感到兴奋，让你觉得你不是一个人在奋斗，让你时刻记住你的前方已经有很多的学者正在等着你，你要加油。他们的经验会让我们少走些冤枉路，能让我们在他们的基础上进一步理解。下面推荐几个我所知道的或者说我了解到的几位牛人博客和几篇文章：

- **pluskid**，真名张弛原，一位技术大牛，毕业于浙江大学，后来出国深造。他的博文质量非常高，深入浅出，其SVM三层境界的讲解让人茅塞顿开，应该给了很多人启发吧，很值得学习。现在的博客网址：[Chiyuan Zhang](http://pluskid.org/about.html)，原博客网址：[Chiyuan Zhang](http://blog.pluskid.org/)
- **Rachel Zhang**，真名张睿卿，很有气质的一位软妹纸，目前是百度深度学习实验室研发工程师，在CSDN中的博客人气绝对屈指可数，算是IT界的一位女中豪杰。博客网址：[CSDN博客-Rachel Zhang](http://blog.csdn.net/abcjennifer)
- **July**，对算法研究独具一格，目前是七月在线科技创始人兼CEO。博客网址：[July](http://blog.csdn.net/v_JULY_v)
- **Jason**，一位国外机器学习爱好者，其博客内容详实，多篇文章被国内机器学习者翻译。博客网址：http://machinelearningmastery.com/blog/
- 一个国外很好的机器学习博客，里面介绍了详细的算法知识，很全面，从感知机、神经网络、决策树、SVM、Adaboost到随机森林、Deep Learning.网址：[A Blog From a Human-engineer-being](http://www.erogol.com/machine-learning/)
- 一篇涵盖许多机器学习资料的文章：[机器学习(Machine Learning)&深度学习(Deep Learning)资料](http://www.open-open.com/lib/view/open1428112201271.html)
- **Edwin Chen**	，机器学习爱好者，博客内容涵盖数学、机器学习和数据科学。分享其中一篇博文：[Choosing a Machine Learning Classifier](http://blog.echen.me/2011/04/27/choosing-a-machine-learning-classifier/)	
- 一篇以前的博文：[A List of Data Science and Machine Learning Resources](http://conductrics.com/data-science-resources/)，有时间好好阅读阅读，对你绝对有帮助。
- [A Few Useful Things to Know about Machine Learning](http://homes.cs.washington.edu/~pedrod/papers/cacm12.pdf),一篇很有帮助的机器学习文章，里面包括了特征选择与模型的简化。
- [The Discipline of Machine Learning](http://www.cs.cmu.edu/~tom/pubs/MachineLearning.pdf)机器学习规则。该文章比较老，2006年发布的，作者是Tom Mitchell，但很有参考价值，其中定义了机器学习的规则。Mitchell在说服CMU总裁为一个百年内都存在的问题建立一个独立的机器学习部门时，也用到了这本书中的观点。希望能对你也有所帮助。
- 分享一个网站：[简书](http://www.jianshu.com/)。


## **国外网站**

如果你想搜索比较新颖的机器学习资料或是文章，可以到以下网站中搜索，里面不仅包括了机器学习的内容，还有许多其它相关领域内容，如数据科学和云计算等。

- InfoWord：http://www.infoworld.com/reviews/
- Kdnuggets：http://www.kdnuggets.com
- Datasciencecentral：http://www.datasciencecentral.com/
- Datascienceplus：http://datascienceplus.com

## **数据科学竞赛**

关于数据分析的竞赛，国内国外都有，下面推荐几个比较火的竞赛网站 ：

- Kaggle比赛，网址：https://www.kaggle.com/
- DataCastle比赛，网站：http://www.pkbigdata.com/
- 阿里大数据竞赛，目前没有消息了，2015年有个【2015天池大数据竞赛】

## **ML相关算法参考**

- 决策树-参考：[decision Tree（Python实现）](http://blog.csdn.net/dream_angel_z/article/details/45965463)
- SVM支持向量机-参考：[pluskid支持向量机三重境界](http://blog.pluskid.org/?page_id=683)
- Adaboost-参考：[组合算法-Adaboost](http://www.csuldw.com/2015/07/05/2015-07-12-Adaboost/)
- Random Forest-参考：[随机森林算法](http://www.cnblogs.com/wentingtu/archive/2011/12/22/2297405.html)
- 朴素贝叶斯算法-参考：[Naive Bayes算法实现](http://blog.csdn.net/dream_angel_z/article/details/46120867)
- 人工神经网络-参考：http://www.cnblogs.com/luxiaoxun/archive/2012/12/10/2811309.html
- Apriori算法-参考地址：[Apriori关联分析](http://www.csuldw.com/2015/06/04/2015-06-04-Apriori/)
- K最近邻算法-参考：[KNN从原理到实现](http://blog.csdn.net/dream_angel_z/article/details/45896449)
- 梯度树提升GTB算法-参考：[Gradient Tree Boosting（或GBRT）](http://blog.csdn.net/dream_angel_z/article/details/48085889)
- K-means聚类-参考：[K-means cluster](http://blog.csdn.net/dream_angel_z/article/details/46343597)
- 组合算法总结-参考：[Ensemble算法总结](http://www.csuldw.com/2015/07/22/2015-07-22%20%20ensemble/)
- EM期望最大算法-参考：[EM算法](http://blog.csdn.net/zouxy09/article/details/8537620)
- Logistic回归-参考：[逻辑回归](http://blog.csdn.net/wangran51/article/details/8892923)
- HMM隐马尔可夫模型，参考:[HMM](http://blog.csdn.net/likelet/article/details/7056068)
- 条件随机场，参考：[CRF](http://www.tanghuangwhu.com/archives/162)
- 随机森林和GBDT，参考：[决策树模型组合之随机森林与GBDT](http://www.cnblogs.com/LeftNotEasy/archive/2011/03/07/1976562.html)
- 特征选择和特征提取，参考：[特征提取与特征选择](http://blog.csdn.net/lanbing510/article/details/40488787)
- 梯度下降法，参考:[gradient descent](http://blog.csdn.net/woxincd/article/details/7040944)
- 牛顿法，参考：[牛顿法](http://blog.csdn.net/luoleicn/article/details/6527049)
- 线性判别分析，参考：[线性判别](http://www.cnblogs.com/jerrylead/archive/2011/04/21/2024384.html)
- 深度学习-[深度学习概述：从感知机到深度网络](http://www.cnblogs.com/xiaowanyer/p/3701944.html)


## **个人译文**

下面是本人在CSDN云计算栏目发布的翻译文章，如有翻译不准确的地方，还望多多包涵，希望能给大家带来点帮助，译文列表如下：

- 2015-09-14 [LSTM实现详解](http://www.csdn.net/article/2015-09-14/2825693)
- 2015-09-10 [从零实现来理解机器学习算法：书籍推荐及障碍的克服](http://www.csdn.net/article/2015-09-08/2825646)
- 2015-08-31  [机器学习开发者的现代化路径：不需要从统计学微积分开始](http://www.csdn.net/article/2015-08-27/2825551)
- 2015-08-27 [基于Python的卷积神经网络和特征提取](http://www.csdn.net/article/2015-08-27/2825549)
- 2015-08-20 [你应该掌握的七种回归技术](http://www.csdn.net/article/2015-08-19/2825492)
- 2015-08-11 [机器学习API Top 10：AT&T Speech、IBM Watson和Google Prediction](http://www.csdn.net/article/2015-08-10/2825430)
- 2015-08-03 [从Theano到Lasagne：基于Python的深度学习的框架和库](http://www.csdn.net/article/2015-08-01/2825362)
- 2015-07-15 [Airbnb欺诈预测机器学习模型设计：准确率和召回率的故事](http://www.csdn.net/article/2015-07-13/2825188)
- 2015-07-13 [开发者成功使用机器学习的十大诀窍](http://www.csdn.net/article/2015-07-13/2825187)

下面是相关译者的译文，仅供参考：

- 2015-09-16 [各种编程语言的深度学习库整理](http://www.csdn.net/article/2015-09-15/2825714)
- 2015-09-11 [机器学习温和指南](http://www.csdn.net/article/2015-09-08/2825647)
- 2015-09-10 [关于数据科学，书上不曾提及的三点经验](http://www.csdn.net/article/2015-09-10/2825668)

---

<font color="#CD3333" >从这些牛人的博客中，你能学到很多。慢慢地你会体会到，不是你一个人在战斗，还有很多人，所以你不用害怕孤独。</font>

最后，关于机器学习资料的整理，先到此为止吧，如果你有什么好的资料，欢迎在评论中给出推荐或网址链接。



---

