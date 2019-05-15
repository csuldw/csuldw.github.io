---
layout: post
date: 2015-11-18 12:24
title: "实验笔记[1]-DSSP文件提取序列"
categories: BioInfo
tag: 
	- BioInfo
	- DSSP
	- 预处理
comment: true
---

__提示：以下内容乃个人实验笔记！__


### 功能描述

从格式化后的dssp文件`DSSP`（单一文件）中提取序列信息，要求输出的序列不含有`X`残基，并且序列最短长度`minlen`可人为指定，一般设置为`40`。

<!--more-->

`DSSP`文件格式Top10:

<pre><code class="markdown">1A12	    1	   21	 A	 K 	          	   0   0  172 	   0 	 172	      0, 0.0	     2,-1.9	     0, 0.0	     0, 0.0	   0.000 	360.0	 360.0	 360.0	 129.7	    9.7	  -11.3	   33.7
1A12	    2	   22	 A	 K 	       -  	   0   0  164 	   0 	 164	      1,-0.1	     2,-0.2	     0, 0.0	     0, 0.0	  -0.433 	360.0	-153.3	 -64.1	  85.0	   10.1	  -13.4	   30.5
1A12	    3	   23	 A	 V 	       -  	   0   0   42 	   0 	  42	     -2,-1.9	     2,-0.2	   114,-0.1	   768,-0.1	  -0.429 	  8.7	-128.1	 -66.5	 129.9	   11.5	  -10.5	   28.4
1A12	    4	   24	 A	 K 	       -  	   0   0  130 	   0 	 130	     -2,-0.2	     2,-0.3	   765,-0.1	   113,-0.3	  -0.476 	 21.9	-164.8	 -79.9	 149.3	   10.9	  -10.9	   24.7
1A12	    5	   25	 A	 V 	       -  	   0   0   13 	   0 	  13	    111,-2.8	     2,-0.2	    -2,-0.2	   113,-0.2	  -0.942 	  6.3	-177.4	-126.4	 157.4	   13.6	  -10.6	   22.1
1A12	    6	   26	 A	 S 	       -  	   0   0   20 	   0 	  20	    719,-1.4	     2,-0.3	    -2,-0.3	   720,-0.1	  -0.694 	  8.2	-153.8	-133.2	-164.5	   13.4	  -10.1	   18.3
1A12	    7	   27	 A	 H 	   >   -  	   0   0    2 	   0 	   2	     -2,-0.2	     3,-1.6	   718,-0.1	   721,-0.3	  -0.944 	 32.3	-112.1	-169.7	 155.9	   15.9	   -9.9	   15.4
1A12	    8	   28	 A	 R 	 T 3  S+  	   0   0   53 	   0 	  53	    718,-0.4	   720,-0.3	   716,-0.3	   717,-0.1	   0.690 	115.2	  60.4	 -69.3	 -20.9	   16.0	   -8.3	   12.0
1A12	    9	   29	 A	 S 	 T 3  S+  	   0   0   34 	   0 	  34	    146,-0.2	    -1,-0.3	   718,-0.1	     2,-0.2	   0.548 	 78.3	 108.7	 -77.8	 -13.9	   15.9	  -11.8	   10.5
1A12	   10	   30	 A	 H 	   <   -  	   0   0   22 	   0 	  22	     -3,-1.6	     2,-0.3	   145,-0.1	    93,-0.1	  -0.516 	 67.8	-130.5	 -73.7	 135.3	   12.6	  -12.8	   12.0
</code></pre>


---

### 代码


`generateSeqFromDSSP.py`文件如下：

```
#!/usr/bin/python
#-*- coding: utf-8 -*-
import os 
'''
Parameters：
    - dsspfile:	为格式过的DSSP文件
    - foseq: 	为输出的序列文件
    - fochain: 	输出的蛋白链文件
    - minLen:  	最短的序列长度
'''
def getSeqFromDSSP(dsspfile, foseq, fochain, minLen):
    with open(dsspfile, 'r') as inputfile:
        if not foseq.strip():
            foseq = 'protein'+ str(minLen) + '.dssp.seq'
        outchain = open(fochain, 'w')
        with open(foseq, 'w') as outputfile:
            residue=[];Ntype=[]
            preType=[];preRes=[]
            firstline=[];secondline=[];content=''
            for eachline in inputfile:
                oneline = eachline.split('\t') 
                residue = oneline[0]
                if not residue.strip(): 
                    continue
                Ntype = oneline[3].strip()
                if not Ntype.strip():
                    continue
                if preRes!=residue:
                    content = ''.join(firstline)+'\n'+''.join(secondline) +'\n'
                    if len(secondline)>=int(minLen) and not 'X' in secondline:
                        outchain.write(''.join(firstline) + '\n')
                        outputfile.write(content)
                    firstline=[]
                    firstline.append('>' + residue + ':' + Ntype)
                    secondline=[];secondline.append(oneline[4].strip())
                    preRes = residue;preType = Ntype
                    continue
                if Ntype != preType:
                    content = ''.join(firstline)+'\n'+''.join(secondline)+'\n'
                    if len(secondline)>=int(minLen) and  not 'X' in secondline:
                        outchain.write(''.join(firstline) + '\n')
                        outputfile.write(content)
                    firstline=[]
                    firstline.append('>' + residue + ':' + Ntype)
                    secondline=[];secondline.append(oneline[4].strip())
                    preRes = residue;preType = Ntype
                else: #如果Ntype不为空，且等于preType
                    secondline.append(oneline[4].strip())
            content = ''.join(firstline)+'\n' + ''.join(secondline) +'\n'
            #选择长度大于40而且序列中不存在‘X’残基的序列
            if len(secondline) >= int(minLen) and not 'X' in secondline:  
                outchain.write(''.join(firstline) + '\n')
                outputfile.write(content)
        outchain.close()
###############################################################################
if __name__=="__main__":
    os.chdir("/ifs/home/liudiwei/DNA_BP/_data/Exp_DBPI/")
    dsspfile = os.sys.argv[1]
    foseq = os.sys.argv[2]
    fochain = os.sys.argv[3]
    minlen = os.sys.argv[4]
    getSeqFromDSSP(dsspfile, foseq, fochain, minlen)
```

---

### Test sample

在Linux控制台中输入下面命令：

```
python generateSeqFromDSSP.py \
/ifs/home/liudiwei/DNA_BP/_data/Exp_DBPI/dssp_testset/DSSP \
/ifs/home/liudiwei/DNA_BP/_data/Exp_DBPI/protein_test.seq \
/ifs/home/liudiwei/DNA_BP/_data/Exp_DBPI/protein_test.chain \
40
```

输出文件：

`protein_test.seq`格式Top10：

<pre><code class="markdown">>1A12:A
KKVKVSHRSHSTEPGLVLTLGQGDVGQLGLGENVMERKKPALVSIPEDVVQAEAGGMHTVCLSKSGQVYSFGCNDEGALGRDTSVEGSEMVPGKVELQEKVVQVSAGDSHTAALTDDGRVFLWGSFRDNNGVIGLLEPMKKSMVPVQVQLDVPVVKVASGNDHLVMLTADGDLYTLGCGEQGQLGRVPELFANRGGRQGLERLLVPKCVMLKSRGSRGHVRFQDAFCGAYFTFAISHEGHVYGFGLSNYHQLGTPGTESCFIPQNLTSFKNSTKSWVGFSGGQHHTVCMDSEGKAYSLGRAEYGRLGLGEGAEEKSIPTLISRLPAVSSVACGASVGYAVTKDGRVFAWGMGTNYQLGTGQDEDAWSPVEMMGKQLENRVVLSVSSGGQHTVLLVKDKEQS
>1A12:B
KKVKVSHRSHSTEPGLVLTLGQGDVGQLGLGENVMERKKPALVSIPEDVVQAEAGGMHTVCLSKSGQVYSFGCNDEGALGRDTSVEGSEMVPGKVELQEKVVQVSAGDSHTAALTDDGRVFLWGSFRDNNGVIGLLEPMKKSMVPVQVQLDVPVVKVASGNDHLVMLTADGDLYTLGCGEQGQLGRVPELFANRGGRQGLERLLVPKCVMLKSRGSRGHVRFQDAFCGAYFTFAISHEGHVYGFGLSNYHQLGTPGTESCFIPQNLTSFKNSTKSWVGFSGGQHHTVCMDSEGKAYSLGRAEYGRLGLGEGAEEKSIPTLISRLPAVSSVACGASVGYAVTKDGRVFAWGMGTNYQLGTGQDEDAWSPVEMMGKQLENRVVLSVSSGGQHTVLLVKDKEQS
>1A12:C
KKVKVSHRSHSTEPGLVLTLGQGDVGQLGLGENVMERKKPALVSIPEDVVQAEAGGMHTVCLSKSGQVYSFGCNDEGALGRDTSVEGSEMVPGKVELQEKVVQVSAGDSHTAALTDDGRVFLWGSFRDNNGVIGLLEPMKKSMVPVQVQLDVPVVKVASGNDHLVMLTADGDLYTLGCGEQGQLGRVPELFANRGGRQGLERLLVPKCVMLKSRGSRGHVRFQDAFCGAYFTFAISHEGHVYGFGLSNYHQLGTPGTESCFIPQNLTSFKNSTKSWVGFSGGQHHTVCMDSEGKAYSLGRAEYGRLGLGEGAEEKSIPTLISRLPAVSSVACGASVGYAVTKDGRVFAWGMGTNYQLGTGQDEDAWSPVEMMGKQLENRVVLSVSSGGQHTVLLVKDKEQS
>1BCH:1
AIEVKLANMEAEINTLKSKLELTNKLHAFSMGKKSGKKFFVTNHERMPFSKVKALaSELRGTVAIPRNAEENKAIQEVAKTSAFLGITDEVTEGQFMYVTGGRLTYSNWKKDQPDDWYGHGLGGGEDbVHIVDNGLWNDISbQASHTAVaEFPA
>1BCH:2
AIEVKLANMEAEINTLKSKLELTNKLHAFSMGKKSGKKFFVTNHERMPFSKVKALcSELRGTVAIPRNAEENKAIQEVAKTSAFLGITDEVTEGQFMYVTGGRLTYSNWKKDQPDDWYGHGLGGGEDdVHIVDNGLWNDISdQASHTAVcEFPA
</code></pre>

`protein_test.chain`文件格式Top10:

<pre><code class="markdown">>1A12:A
>1A12:B
>1A12:C
>1BCH:1
>1BCH:2
>1BCH:3
>1BF6:B
>1BYP:A
>1C7J:A
>1CHM:A
<code></pre>

---

