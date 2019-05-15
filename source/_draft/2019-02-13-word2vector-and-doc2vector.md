---
layout: post
date: 2019-02-13 12:31
title: "word2vector实践"
categories: ML
tag:
	- word2vector
	- 深度学习
  	- Embedding
  	- Keras
comment: true
---

### Introduction

```
pip install gensim
```


gensim中封装了包括了word2vec, doc2vec等模型，word2vec采用了CBOW(Continuous Bag-Of-Words，连续词袋模型)和Skip-Gram两种模型。

<!-- more -->

### 

```
from gensim.models import Word2Vec
model = Word2Vec(sentences, sg=1, size=100,  window=5,  min_count=5,  negative=3, sample=0.001, hs=1, workers=4)
```

1. sentences: 第一个参数是预处理后的训练语料库。是可迭代列表，但是对于较大的语料库，可以考虑直接从磁盘/网络传输句子的迭代。
2. sg=1是skip-gram算法，对低频词敏感；默认sg=0为CBOW算法。
3. size(int): 是输出词向量的维数，默认值是100。这个维度的取值与我们的语料的大小相关，比如小于100M的文本语料，则使用默认值一般就可以了。如果是超大的语料，建议增大维度。值太小会导致词映射因为冲突而影响结果，值太大则会耗内存并使算法计算变慢，一般值取为100到200之间，不过见的比较多的也有300维的。
4. window（int）:一个句子中当前单词和预测单词之间的最大距离，window越大，则和某一词较远的词也会产生上下文关系。默认值为5。windows越大所需要枚举的预测此越多，计算的时间越长。
5. min_count: 忽略所有频率低于此值的单词。默认值为5。
6. workers: 表示训练词向量时使用的线程数,默认是当前运行机器的处理器核数。

还有关采样和学习率的，一般不常设置

1. negative和sample可根据训练结果进行微调，sample表示更高频率的词被随机下采样到所设置的阈值，默认值为1e-3。
2. hs=1表示层级softmax将会被使用，默认hs=0且negative不为0，则负采样将会被选择使用。


### 模型训练保存与加载1（模型可继续训练）

```
使用以下命令初始化模型
from gensim.test.utils import common_texts, get_tmpfile
from gensim.models import Word2Vec
 
path = get_tmpfile("word2vec.model") #创建临时文件
 
model = Word2Vec(sentences, size=100, window=5, min_count=1, workers=4)
model.save("word2vec.model")
 
#加载模型
model = Word2Vec.load("word2vec.model")
```

### References

