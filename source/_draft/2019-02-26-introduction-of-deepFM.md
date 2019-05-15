---
layout: post
date: 2019-02-26 00:01
title: "DeepFM理论"
categories: 推荐系统
tag: 
	- Machine Learning
	- FM
	- 推荐系统
	- Recommendation System
	- Factorization Machines
comment: true
---

## Wide&Deep

Wide&Deep是LR和DNN的结合，其LR部分一般需要人工特征工程以体现特征的组合。

## DeepFM

DeepFM将wide&deep的LR部分替换成FM，可以自动表征特征间的2阶组合。

DeepFM的是由FM+DNN构成

首先会有一个Dense Embedding层，对每个filed通过FM训练得到embedding的权值向量V，其中Vij表示第i个特征embedding之后的隐向量的第j维值，

FM部分和Deep部分共享相同的输入。Wide&Deep中LR部分和Deep部分是完全独立的，LR部分的输入是稀疏特征，Deep部分稀疏特征先经过embeding层变成embeding向量，再传入隐层。DeepFM中每一个feature field经过embeding层转化为一个隐向量，多个特征field concat成一个密集向量分别作为FM部分和Deep部分的输入。FM部分将每个field的隐向量两两组合，最后在输出层和Deep部分输出concat成最终的输出层。

FM部分自动表征特征域的2阶组合。Wide&Deep不能体现特征间的组合型，准确的说是不能体现特征间的乘性组合（Deep部分能体现特征域间的加性组合）。需要人工特征工程来完成特征组合。

Wide&Deep和DeepFM都提供了端到端的解决方案，不需要pre-training；同时都能体现低阶和高阶特征。但是Wide&Deep需要人工特征工程，DeepFM不需要。因此，在实际工作中，DeepFM比Wide&Deep更加高效。

Wide&Deep、DeepFM、DCN都是传统CTR模型与DNN相结合。DeepFM相比Wide&Deep不需要人工特征工程，具有一定优势。DCN相比DeepFM能模拟任意阶特征组合，灵活性更好。当DCN的cross layer深度为1，cross部分表征二阶组合特征，进行某种加权后再传入输出层，而DeepFM直接将所有交叉特征concat后传入输出层。DCN和DeepFM究竟哪个更有效，可能还需要更多的数据和实验支撑。
## References

1. [DeepFM: A Factorization-Machine based Neural Network for CTR Prediction](https://arxiv.org/pdf/1703.04247.pdf)
2. [Wide & Deep Learning for Recommender Systems](https://arxiv.org/pdf/1606.07792.pdf)
3. https://zhuanlan.zhihu.com/p/40807531
4. https://blog.csdn.net/google19890102/article/details/78171283
5. https://www.cnblogs.com/arachis/p/FTRL.html