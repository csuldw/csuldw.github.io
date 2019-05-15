---
layout: post
date: 2019-02-11 23:22
title: "基于item-Embedding的book推荐"
categories: ML
tag:
	- 推荐系统
	- 深度学习
  	- Embedding
  	- Keras
comment: true
---


<!-- more -->

### Dataset

数据集：[goodbooks-10k](https://www.kaggle.com/zygmunt/goodbooks-10k/kernels?sortBy=hotness&group=everyone&pageSize=20&datasetId=1938)

### 冷启动介绍



### Experiment

1.导入相关库

```
import os
import pandas as pd
import numpy as np 
import keras

from keras.preprocessing import sequence
from sklearn.cross_validation import train_test_split
from keras.models import Sequential
from keras.layers.embeddings import Embedding
from keras.layers.recurrent import LSTM
from keras.layers.core import Dense, Dropout,Activation
from keras.models import model_from_yaml
```

2.加载数据集

```
dataset = pd.read_csv('ratings.csv')
train, test = train_test_split(dataset, test_size=0.2, random_state=42)
n_users = len(dataset.user_id.unique())
n_books = len(dataset.book_id.unique())
dataset.head()
```

3.建立模型

```
from keras.layers import Input, Embedding, Flatten, Dot, Dense
from keras.models import Model

book_input = Input(shape=[1], name="Book-Input")
book_embedding = Embedding(n_books+1, 5, name="Book-Embedding")(book_input)
book_vec = Flatten(name="Flatten-Books")(book_embedding)

user_input = Input(shape=[1], name="User-Input")
user_embedding = Embedding(n_users+1, 5, name="User-Embedding")(user_input)
user_vec = Flatten(name="Flatten-Users")(user_embedding)

prod = Dot(name="Dot-Product", axes=1)([book_vec, user_vec])
model = Model([user_input, book_input], prod)
model.compile('adam', 'mean_squared_error')

model.summary()
```

4.训练回归模型

```
history = model.fit([train.user_id, train.book_id], train.rating, epochs=1, verbose=1)
model.save('regression_model.h5')
```

5.查看embeddings

```
# Extract embeddings
book_em = model.get_layer('Book-Embedding')
book_em_weights = book_em.get_weights()[0]
book_em_weights
```

6.PCA可视化

```
from sklearn.decomposition import PCA
import seaborn as sns


pca = PCA(n_components=2)
pca_result = pca.fit_transform(book_em_weights)
sns.jointplot(x=pca_result[:,0], y=pca_result[:,1])
```


```
from sklearn.manifold import TSNE

tsne = TSNE(n_components=2, verbose=1, perplexity=40, n_iter=300)
tnse_results = tsne.fit_transform(book_em_weights)
sns.jointplot(x=tnse_results[:,0], y=tnse_results[:,1])
```



7.用户推荐


```
# Creating dataset for making recommendations for the first user
book_data = np.array(list(set(dataset.book_id)))
user = np.array([1 for i in range(len(book_data))])
predictions = model.predict([user, book_data])
predictions = np.array([a[0] for a in predictions])
recommended_book_ids = (-predictions).argsort()[:30]
print(recommended_book_ids)
print(predictions[recommended_book_ids])
```


### References

- [用 Keras 实现图书推荐系统](https://ai.yanxishe.com/page/TextTranslation/1300)
- [ASPEM: Embedding Learning by Aspects in Heterogeneous Information Networks](http://yushi2.web.engr.illinois.edu/sdm18.pdf)
- [Distributed Representations of Words and Phrases and their Compositionality](https://papers.nips.cc/paper/5021-distributed-representations-of-words-and-phrases-and-their-compositionality.pdf)
- [Extracting and Composing Robust Features with Denoising Autoencoders](http://www.cs.toronto.edu/~larocheh/publications/icml-2008-denoising-autoencoders.pdf)
- [GloVe: Global Vectors for Word Representation](https://nlp.stanford.edu/projects/glove/)
- [building a book recommendation system using keras](https://towardsdatascience.com/building-a-book-recommendation-system-using-keras-1fba34180699)
