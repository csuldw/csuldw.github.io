---
layout: post
date: 2015-11-03 16:24
title: "Add header and footer to some file"
categories: Python
tag: 
	- Python
comment: true
---

今天整理资料的时候，发现要在很多文件中的头部和尾部添加相同的文本，于是自己使用Python做了一个简单的文件拼接功能，也可以说是文件追加功能，给一个文件批量追加头尾内容，达到省事的效果，顺便还可以练习下Python。下面来介绍下这个功能的代码：

现在有三个文件，如下：

- content.txt 位于一个叫path的文件中；
- header.txt用于添加到content.txt头部的文件；
- footer.txt用于添加到content.txt尾部的文件。


现在要实现的功能就是，将header和footer分别添加到content的头部和尾部。 

<!--more-->

---

函数说明：

- add_footer(infile, outfile)：用于将footer内容添加到content中，第一个参数表示的添加到尾部的文件，如输入footer.txt，第二个为内容文件。如content.txt文件
- add_header(infile, outfile, auto=True): 用于将一个文件放入好另一个文件的头部，如果auto=Ture，则不对内容做修改，auto为False的话，这里添加了部分需要的东西，如文件的创建时间、标题等信息。
- addHeadAndFooter(path, header, footer, auto=False)：核心函数，调用头尾两个方法，此处的path为文件夹名称，该函数的功能是将path文件夹下的所有文件都添加头和尾的内容，auto默认为False，功能和上面的相同。
- getStdTime(seconds):将时间戳格式的日期转换为标准格式，如：2015-11-03 10:24


代码（AddHeader.py）：


```
# -*- coding: utf-8 -*-
"""
Created on Tue Nov 03 10:32:26 2015
@author: liudiwei
"""
import os,time
def add_footer(infile, outfile):
    with open(infile,'r') as inputfile:
        with open(outfile,'a') as outfile:
            outfile.write("\n\n"+''.join(inputfile.readlines()))
#如果auto==True，直接将文件内容加入到当前文件
def add_header(infile, outfile, auto=True): 
    inf=open(infile,'r')
    outf = open(outfile,'r')
    header = inf.readlines()
    content=outf.readlines()
    if auto==True:
        with open(outfile,'w') as output:
            output.write(''.join(header)+ "\n\n" \
                            +''.join(content))  
    else:
        ctime=getStdTime(os.path.getctime(outfile))
        title="title: " + outfile.split('/')[1].split('.')[0]
        print title
        add_content="---\n"
        add_content=add_content+title+'\n'  #add title
        add_content=add_content+ctime +'\n' #add date
        add_content=add_content+''.join(header)
        with open(outfile,'w') as output:
            output.write(''.join(add_content)+ "\n\n" \
                        +''.join(content))  
    outf.close()
    inf.close()
def addHeadAndFooter(path, header, footer, auto=False):
    filelist=os.listdir(path)
    for eachfile in filelist:
        add_header(header,path + "/" + eachfile, auto)
        add_footer(footer,path + "/" + eachfile)       
def getStdTime(seconds):
    x = time.localtime(seconds)
    return "date: "+ time.strftime('%Y-%m-%d %H:%M:%S',x)        
if __name__=='__main__':
    if (len(os.sys.argv)<4):
        raise TypeError()
    else:
        print "os.sys.arg"
    #path="path"
    #header="head.md"
    #footer="footer.md"
    os.chdir(".")
    path=os.sys.argv[1]
    print path
    header=os.sys.argv[2]
    footer=os.sys.argv[3]
    filelist=os.listdir(path)
    addHeadAndFooter(path,header,footer)
    print "Success added!"    
#----------------    
# command 
# python AddHead.py "path" "header.txt" "footer.txt"
#----------------
```

直接在console控制台上运行下列代码即可 

```
python AddHeader.py "path" "header.txt" "footer.txt"
```



---



