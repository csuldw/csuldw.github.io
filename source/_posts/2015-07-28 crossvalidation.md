---
layout: post
date: 2015-07-28 15:40
title: "机器学习-Cross Validation交叉验证Python实现"
tags: 
	- Machine Learning
	- 交叉验证
	- Cross-Validation
categories: ML
---

## __1.原理__

### **1.1 概念**

交叉验证(Cross-validation)主要用于模型训练或建模应用中，如分类预测、PCR、PLS回归建模等。在给定的样本空间中，拿出大部分样本作为训练集来训练模型，剩余的小部分样本使用刚建立的模型进行预测，并求这小部分样本的预测误差或者预测精度，同时记录它们的加和平均值。这个过程迭代K次，即K折交叉。其中，把每个样本的预测误差平方加和，称为PRESS(predicted Error Sum of Squares)。
<!-- more -->

### **1.2 目的**

用交叉验证的目的是为了得到可靠稳定的模型。在分类，建立PC 或PLS模型时，一个很重要的因素是取多少个主成分的问题。用cross validation校验每个主成分下的PRESS值，选择PRESS值小的主成分数。或PRESS值不再变小时的主成分数。

常用的精度测试方法主要是交叉验证，例如10折交叉验证(10-fold cross validation)，将数据集分成十份，轮流将其中9份做训练1份做验证，10次的结果的均值作为对算法精度的估计，一般还需要进行多次10折交叉验证求均值，例如：10次10折交叉验证，以求更精确一点。
交叉验证有时也称为交叉比对，如：10折交叉比对

### **1.3 常见的交叉验证形式**：

__Holdout 验证__

>方法：将原始数据随机分为两组,一组做为训练集,一组做为验证集,利用训练集训练分类器,然后利用验证集验证模型,记录最后的分类准确率为此Hold-OutMethod下分类器的性能指标.。Hold-OutMethod相对于K-fold Cross Validation 又称Double cross-validation ，或相对K-CV称 2-fold cross-validation(2-CV)

>一般来说，Holdout 验证并非一种交叉验证，因为数据并没有交叉使用。 随机从最初的样本中选出部分，形成交叉验证数据，而剩余的就当做训练数据。 一般来说，少于原本样本三分之一的数据被选做验证数据。

- 优点：好处的处理简单,只需随机把原始数据分为两组即可
- 缺点：严格意义来说Hold-Out Method并不能算是CV,因为这种方法没有达到交叉的思想,由于是随机的将原始数据分组,所以最后验证集分类准确率的高低与原始数据的分组有很大的关系,所以这种方法得到的结果其实并不具有说服性.(主要原因是 训练集样本数太少，通常不足以代表母体样本的分布，导致 test 阶段辨识率容易出现明显落差。此外，2-CV 中一分为二的分子集方法的变异度大，往往无法达到「实验过程必须可以被复制」的要求。)

__K-fold cross-validation__

>K折交叉验证，初始采样分割成K个子样本，一个单独的子样本被保留作为验证模型的数据，其他K-1个样本用来训练。交叉验证重复K次，每个子样本验证一次，平均K次的结果或者使用其它结合方式，最终得到一个单一估测。这个方法的优势在于，同时重复运用随机产生的子样本进行训练和验证，每次的结果验证一次，10折交叉验证是最常用的。

- 优点：K-CV可以有效的避免过学习以及欠学习状态的发生,最后得到的结果也比较具有说服性.  
- 缺点：K值选取上

__留一验证__

>正如名称所建议， 留一验证（LOOCV）意指只使用原本样本中的一项来当做验证资料， 而剩余的则留下来当做训练资料。 这个步骤一直持续到每个样本都被当做一次验证资料。 事实上，这等同于 K-fold 交叉验证是一样的，其中K为原本样本个数。 在某些情况下是存在有效率的演算法，如使用kernel regression 和Tikhonov regularization。
 
## __2.深入__

使用交叉验证方法的目的主要有3个： 

- （1）从有限的学习数据中获取尽可能多的有效信息； 
- （2）交叉验证从多个方向开始学习样本的，可以有效的避免陷入局部最小值； 
- （3）可以在一定程度上避免过拟合问题。

采用交叉验证方法时需要将学习数据样本分为两部分：训练数据样本和验证数据样本。并且为了得到更好的学习效果，无论训练样本还是验证样本都要尽可能参与学习。一般选取10重交叉验证即可达到好的学习效果。下面在上述原则基础上设计算法，主要描述下算法步骤，如下所示。


Algorithm  
------------------------------------------------
```
Step1: 	将学习样本空间 C 分为大小相等的 K 份  
Step2: 	for i = 1 to K ：
			取第i份作为测试集
			for j = 1 to K:
				if i != j:
					将第j份加到训练集中，作为训练集的一部分
				end if
			end for
		end for
Step3: 	for i in (K-1训练集)：
			训练第i个训练集，得到一个分类模型
			使用该模型在第N个数据集上测试，计算并保存模型评估指标
		end for
Step4: 	计算模型的平均性能
Step5: 	用这K个模型在最终验证集的分类准确率平均值作为此K-CV下分类器的性能指标.
```

## __3.实现__

### __3.1 scikit-learn交叉验证__

在scikit-learn中有CrossValidation的实现代码，地址： [scikit-learn官网crossvalidation文档](http://scikit-learn.org/dev/modules/cross_validation.html#cross-validation)

使用方法：

首先加载数据集

```
>>> import numpy as np
>>> from sklearn import cross_validation
>>> from sklearn import datasets
>>> from sklearn import svm
>>> iris = datasets.load_iris()
>>> iris.data.shape, iris.target.shape
((150, 4), (150,))
```

通过上面代码，数据集特征和类标签分别为iris.data, iris.target，接着进行交叉验证

```
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

上面的clf是分类器，可以自己替换，比如我可以使用RandomForest

```
clf = RandomForestClassifier(n_estimators=400)
```

一个比较有用的函数是train_test_split。功能是从样本中随机的按比例选取train data和test data。形式为

```
X_train, X_test, y_train, y_test = cross_validation.train_test_split(train_data,train_target, test_size=0.4, random_state=0)
```
test_size是样本占比。如果是整数的话就是样本的数量。random_state是随机数的种子。

当然，也可以换成别的，具体算法可以参考 [scikit-learn官方文档](http://scikit-learn.org/dev/supervised_learning.html#supervised-learning)

-------------------------

### __3.2 抽样与CV结合__

> 由于我跑的实验，数据是非均衡数据，不能直接套用，所以这里自己写了一个交叉验证的代码，仅供参考，如有问题，欢迎交流。

首先有一个自适应的数据加载函数，主要用于加载本地文本数据，同时文本每行数据以"\t"隔开，最后一列为类标号，数据样例如下：

```
A1001	708	K	-4	-3	6	2	-13	0	2	-4	-4	-10	-9	1
A1002	709	L	-4	-4	-1	-2	-11	-1	0	-12	-7	-5	-1	-1
A1003	710	G	0	-6	-2	-6	-8	-4	-6	-6	-9	-4	0	-1
A1004	711	R	0	0	1	-3	-10	-1	-3	-4	-6	-9	-6	1
```

**说明**：前面三个不是特征，所以在加载数据集的时候，特征部分起始位置修改了下，loadDataSet函数如下：

```
def loadDataSet(fileName):
    fr = open(fileName)
    dataMat = []; labelMat = []
    for eachline in fr:
        lineArr = []
        curLine = eachline.strip().split('\t') #remove '\n'
        for i in range(3, len(curLine)-1):
            lineArr.append(float(curLine[i])) #get all feature from inpurfile
        dataMat.append(lineArr)
        labelMat.append(int(curLine[-1])) #last one is class lable
    fr.close()
    return dataMat,labelMat
```

返回的dataMat为纯特征矩阵，labelMat为类别标号。

下面的**splitDataSet**用来切分数据集，如果是十折交叉，则split_size取10，filename为整个数据集文件，outdir则是切分的数据集的存放路径。

```
def splitDataSet(fileName, split_size,outdir):
    if not os.path.exists(outdir): #if not outdir,makrdir
        os.makedirs(outdir)
    fr = open(fileName,'r')#open fileName to read
    num_line = 0
    onefile = fr.readlines()
    num_line = len(onefile)        
    arr = np.arange(num_line) #get a seq and set len=numLine
    np.random.shuffle(arr) #generate a random seq from arr
    list_all = arr.tolist()
    each_size = (num_line+1) / split_size #size of each split sets
    split_all = []; each_split = []
    count_num = 0; count_split = 0  #count_num 统计每次遍历的当前个数
                                    #count_split 统计切分次数
    for i in range(len(list_all)): #遍历整个数字序列
        each_split.append(onefile[int(list_all[i])].strip()) 
        count_num += 1
        if count_num == each_size:
            count_split += 1 
            array_ = np.array(each_split)
            np.savetxt(outdir + "/split_" + str(count_split) + '.txt',\
                        array_,fmt="%s", delimiter='\t')  #输出每一份数据
            split_all.append(each_split) #将每一份数据加入到一个list中
            each_split = []
            count_num = 0
    return split_all
```

underSample(datafile)方法为抽样函数，强正负样本比例固定为1:1，返回的是一个正负样本比例均等的数据集合。

```
def underSample(datafile): #只针对一个数据集的下采样
    dataMat,labelMat = loadDataSet(datafile) #加载数据
    pos_num = 0; pos_indexs = []; neg_indexs = []   
    for i in range(len(labelMat)):#统计正负样本的下标    
        if labelMat[i] == 1:
            pos_num +=1
            pos_indexs.append(i)
            continue
        neg_indexs.append(i)
    np.random.shuffle(neg_indexs)
    neg_indexs = neg_indexs[0:pos_num]
    fr = open(datafile, 'r')
    onefile = fr.readlines()
    outfile = []
    for i in range(pos_num):
        pos_line = onefile[pos_indexs[i]]    
        outfile.append(pos_line)
        neg_line= onefile[neg_indexs[i]]      
        outfile.append(neg_line)
    return outfile #输出单个数据集采样结果
```

下面的generateDataset(datadir,outdir)方法是从切分的数据集中留出一份作为测试集（无需抽样），对其余的进行抽样然后合并为一个作为训练集，代码如下：

```
def generateDataset(datadir,outdir): #从切分的数据集中，对其中九份抽样汇成一个,\
    #剩余一个做为测试集,将最后的结果按照训练集和测试集输出到outdir中
    if not os.path.exists(outdir): #if not outdir,makrdir
        os.makedirs(outdir)
    listfile = os.listdir(datadir)
    train_all = []; test_all = [];cross_now = 0
    for eachfile1 in listfile:
        train_sets = []; test_sets = []; 
        cross_now += 1 #记录当前的交叉次数
        for eachfile2 in listfile:
            if eachfile2 != eachfile1:#对其余九份欠抽样构成训练集
                one_sample = underSample(datadir + '/' + eachfile2)
                for i in range(len(one_sample)):
                    train_sets.append(one_sample[i])
        #将训练集和测试集文件单独保存起来
        with open(outdir +"/test_"+str(cross_now)+".datasets",'w') as fw_test:
            with open(datadir + '/' + eachfile1, 'r') as fr_testsets:
                for each_testline in fr_testsets:                
                    test_sets.append(each_testline) 
            for oneline_test in test_sets:
                fw_test.write(oneline_test) #输出测试集
            test_all.append(test_sets)#保存训练集
        with open(outdir+"/train_"+str(cross_now)+".datasets",'w') as fw_train:
            for oneline_train in train_sets:   
                oneline_train = oneline_train
                fw_train.write(oneline_train)#输出训练集
            train_all.append(train_sets)#保存训练集
    return train_all,test_all
```

因为需要评估交叉验证，所以我写了一个performance方法根据真实类标签纸和预测值来计算SN和SP，当然如果需要其他的评估标准，继续添加即可。

```
def performance(labelArr, predictArr):#类标签为int类型
    #labelArr[i] is actual value,predictArr[i] is predict value
    TP = 0.; TN = 0.; FP = 0.; FN = 0.   
    for i in range(len(labelArr)):
        if labelArr[i] == 1 and predictArr[i] == 1:
            TP += 1.
        if labelArr[i] == 1 and predictArr[i] == -1:
            FN += 1.
        if labelArr[i] == -1 and predictArr[i] == 1:
            FP += 1.
        if labelArr[i] == -1 and predictArr[i] == -1:
            TN += 1.
    SN = TP/(TP + FN) #Sensitivity = TP/P  and P = TP + FN 
    SP = TN/(FP + TN) #Specificity = TN/N  and N = TN + FP
    #MCC = (TP*TN-FP*FN)/math.sqrt((TP+FP)*(TP+FN)*(TN+FP)*(TN+FN))
    return SN,SP
```

 classifier(clf,train_X, train_y, test_X, test_y)方法是交叉验证中每次用的分类器训练过程以及测试过程，里面使用的分类器是scikit-learn自带的。该方法会将一些训练结果写入到文件中并保存到本地，同时在最后会返回ACC,SP,SN。

```
def classifier(clf,train_X, train_y, test_X, test_y):#X:训练特征，y:训练标号
    # train with randomForest 
    print " training begin..."
    clf = clf.fit(train_X,train_y)
    print " training end."
    #==========================================================================
    # test randomForestClassifier with testsets
    print " test begin."
    predict_ = clf.predict(test_X) #return type is float64
    proba = clf.predict_proba(test_X) #return type is float64
    score_ = clf.score(test_X,test_y)
    print " test end."
    #==========================================================================
    # Modeal Evaluation
    ACC = accuracy_score(test_y, predict_)
    SN,SP = performance(test_y, predict_)
    MCC = matthews_corrcoef(test_y, predict_)
    #AUC = roc_auc_score(test_labelMat, proba)
    #==========================================================================
    #save output 
    eval_output = []
    eval_output.append(ACC);eval_output.append(SN)  #eval_output.append(AUC)
    eval_output.append(SP);eval_output.append(MCC)
    eval_output.append(score_)
    eval_output = np.array(eval_output,dtype=float)
    np.savetxt("proba.data",proba,fmt="%f",delimiter="\t")
    np.savetxt("test_y.data",test_y,fmt="%f",delimiter="\t")
    np.savetxt("predict.data",predict_,fmt="%f",delimiter="\t") 
    np.savetxt("eval_output.data",eval_output,fmt="%f",delimiter="\t")
    print "Wrote results to output.data...EOF..."
    return ACC,SN,SP
```

下面的mean_fun用于求列表list中数值的平均值，主要是求ACC_mean,SP_mean,SN_mean，用来评估模型好坏。

```
def mean_fun(onelist):
    count = 0
    for i in onelist:
        count += i
    return float(count/len(onelist))
```

交叉验证代码

```
def crossValidation(clf, clfname, curdir,train_all, test_all):
    os.chdir(curdir)
    #构造出纯数据型样本集
    cur_path = curdir
    ACCs = [];SNs = []; SPs =[]
    for i in range(len(train_all)):
        os.chdir(cur_path)
        train_data = train_all[i];train_X = [];train_y = []
        test_data = test_all[i];test_X = [];test_y = []
        for eachline_train in train_data:
            one_train = eachline_train.split('\t') 
            one_train_format = []
            for index in range(3,len(one_train)-1):
                one_train_format.append(float(one_train[index]))
            train_X.append(one_train_format)
            train_y.append(int(one_train[-1].strip()))
        for eachline_test in test_data:
            one_test = eachline_test.split('\t')
            one_test_format = []
            for index in range(3,len(one_test)-1):
                one_test_format.append(float(one_test[index]))
            test_X.append(one_test_format)
            test_y.append(int(one_test[-1].strip()))
        #======================================================================
        #classifier start here
        if not os.path.exists(clfname):#使用的分类器
            os.mkdir(clfname)
        out_path = clfname + "/" + clfname + "_00" + str(i)#计算结果文件夹
        if not os.path.exists(out_path):
            os.mkdir(out_path)
        os.chdir(out_path)
        ACC, SN, SP = classifier(clf, train_X, train_y, test_X, test_y)
        ACCs.append(ACC);SNs.append(SN);SPs.append(SP)
        #======================================================================
    ACC_mean = mean_fun(ACCs)
    SN_mean = mean_fun(SNs)
    SP_mean = mean_fun(SPs)
    #==========================================================================
    #output experiment result
    os.chdir("../")
    os.system("echo `date` '" + str(clf) + "' >> log.out")
    os.system("echo ACC_mean=" + str(ACC_mean) + " >> log.out")
    os.system("echo SN_mean=" + str(SN_mean) + " >> log.out")
    os.system("echo SP_mean=" + str(SP_mean) + " >> log.out")
    return ACC_mean, SN_mean, SP_mean
```

**测试：**

```python
if __name__ == '__main__':
	os.chdir("your workhome") #你的数据存放目录
    datadir = "split10_1" #切分后的文件输出目录
    splitDataSet('datasets',10,datadir)#将数据集datasets切为十个保存到datadir目录中
	#==========================================================================
    outdir = "sample_data1"	#抽样的数据集存放目录
    train_all,test_all = generateDataset(datadir,outdir) #抽样后返回训练集和测试集
    print "generateDataset end and cross validation start"
    #==========================================================================
    #分类器部分
    from sklearn.ensemble import RandomForestClassifier
    clf = RandomForestClassifier(n_estimators=500) #使用随机森林分类器来训练
    clfname = "RF_1"
    #==========================================================================
    curdir = "experimentdir" #工作目录
	#clf:分类器,clfname:分类器名称,curdir:当前路径,train_all:训练集,test_all:测试集
    ACC_mean, SN_mean, SP_mean = crossValidation(clf, clfname, curdir, train_all,test_all)
    print ACC_mean,SN_mean,SP_mean	#将ACC均值，SP均值，SN均值都输出到控制台
```

上面的代码主要用于抽样后的十倍交叉验证，该怎么设置参数，还得具体分析。

总之，交叉验证在一定程度上能够避免陷入局部最小值。一般实际操作中使用的是十折交叉验证，单具体情况还得具体分析，并没有一个统一的标准固定十倍交叉的参数或者是算法的选择以及算法参数的选择。不同的数据使用不同的算法往往会的得到不同的最优分类器。So,just try it!Happy coding!

------
<br>
