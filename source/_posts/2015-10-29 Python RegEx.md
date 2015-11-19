---
layout: post
date: 2015-10-29 20:24
title: "Python-RegEx（正则表达式）"
categories: Python
tag: 
	- Python
	- 正则表达式
comment: true
---

关于Python的正则表达式，初步学习了下，感觉跟shell脚本的正则表达式大体相同，先来做个小结吧！

## 正则表达式简介

正则表达式在实际的文本文件处理中，经常用到，其实正则表达式并不是Python的一部分，其它语言中都有。正则表达式是用于处理字符串的强大工具，拥有自己独特的语法以及一个独立的处理引擎，效率上可能不如str自带的方法，但功能真的十分强大。得益于这一点，在提供了正则表达式的语言里，正则表达式的语法都是一样的，区别只在于不同的编程语言实现支持的语法数量不同；但不用担心，不被支持的语法通常是不常用的部分。如果已经在其他语言里使用过正则表达式，只需要简单看一看就可以上手了。

下图展示了使用正则表达式进行匹配的流程： 

![](http://ww4.sinaimg.cn/large/637f3c58gw1exic0q7k4ej20cj055t9e.jpg)

<!-- more -->

从上图我们可以看出，正则表达式的大致匹配过程是：依次拿出表达式和文本中的字符比较，如果每一个字符都能匹配，则匹配成功；一旦有匹配不成功的字符则匹配失败。如果表达式中有量词或边界，这个过程会稍微有一些不同，但也是很好理解的，来看看下面的这个正则表达式模式。

|模式|描述|  
|--------------|----|
|^|	匹配字符串的开头|
|$|	匹配字符串的末尾。|
|.|	匹配任意字符，除了换行符，当re.DOTALL标记被指定时，则可以匹配包括换行符的任意字符。|
|[...]|	用来表示一组字符,单独列出：[amk] 匹配 'a'，'m'或'k'|
|[^...]|	不在[]中的字符：[^abc] 匹配除了a,b,c之外的字符。|
|*|	匹配0个或多个的表达式。|
|+|	匹配1个或多个的表达式。|
|?|	 匹配0个或1个由前面的正则表达式定义的片段，非贪婪方式|
|{ n,}|	精确匹配n个前面表达式。|
|{ n, m}|	匹配 n 到 m 次由前面的正则表达式定义的片段，贪婪方式|
|(re)|	G匹配括号内的表达式，也表示一个组|
|(?imx)|	正则表达式包含三种可选标志：i, m, 或 x 。只影响括号中的区域。|
|(?-imx)|	正则表达式关闭 i, m, 或 x 可选标志。只影响括号中的区域。|
|(?: re)|	 类似 (...), 但是不表示一个组|
|(?imx: re)|	在括号中使用i, m, 或 x 可选标志|
|(?-imx: re)|	在括号中不使用i, m, 或 x 可选标志|
|(?#...)|	注释.|
|(?= re)|	前向肯定界定符。如果所含正则表达式，以 ... 表示，在当前位置成功匹配时成功，否则失败。但一旦所含表达式已经尝试，匹配引擎根本没有提高；模式的剩余部分还要尝试界定符的右边。|
|(?! re)|	前向否定界定符。与肯定界定符相反；当所含表达式不能在字符串当前位置匹配时成功|
|(?> re)|	匹配的独立模式，省去回溯。|
|\w|	匹配字母数字,等价于'[A-Za-z0-9_]'|
|\W|	匹配非字母数字, [^A-Za-z0-9_]'|
|\s|	匹配任意空白字符，等价于[\t\n\r\f].|
|\S|	匹配任意非空字符,等价于[^ \f\n\r\t\v]|
|\d|	匹配任意数字，等价于[0-9].|
|\D|	匹配任意非数字,等价于[^0-9]。|
|\A|	匹配字符串开始|
|\Z|	匹配字符串结束，如果是存在换行，只匹配到换行前的结束字符串。c|
|\z|	匹配字符串结束|
|\G|	匹配最后匹配完成的位置。|
|\b|	匹配一个单词边界，也就是指单词和空格间的位置。例如， 'er\b' 可以匹配"never" 中的 'er'，但不能匹配 "verb" 中的 'er'。|
|\B|	匹配非单词边界。'er\B' 能匹配 "verb" 中的 'er'，但不能匹配 "never" 中的 'er'。|
|\n, \t, 等.|	匹配一个换行符。匹配一个制表符。等|
|\1...\9|	匹配第n个分组的子表达式。|
|\10|	匹配第n个分组的子表达式，如果它经匹配。否则指的是八进制字符码的表达式。|


下面从正则表达式的几个函数/方法来简单介绍下正则表达式的用法。

---

## re.match函数

re.match 尝试**从字符串的开头匹配一个模式**，如：下面的例子匹配第一个单词。 

```
import re
text = "This is a very beautiful girl, I like her very much."
m = re.match(r"(\w+)\s", text)
if m:
	print m.group(0), '\n', m.group(1)
else:
	print 'not match'  
```

输出:
<pre><code class="markdown">
This
This
</code></pre>

re.match的函数原型为：re.match(pattern, string, flags)

* 第一个参数是正则表达式，这里为"(\w+)\s"，如果匹配成功，则返回一个Match，否则返回一个None；
* 第二个参数表示要匹配的字符串；
* 第三个参数是标致位，用于控制正则表达式的匹配方式，如：是否区分大小写，多行匹配等等。

---


## re.search函数

re.search函数会在字符串内查找模式匹配,只到找到第一个匹配然后返回，如果字符串没有匹配，则返回None。


```
import re
text = "This is a very beautiful girl, I like her very much."
m = re.search(r'\sbeaut(i)ful\s', text)
if m:
	print m.group(0), m.group(1)
else:
	print 'not search' 
```

输出结果：

<pre><code class="markdown">
beautiful i
</code></pre>

re.search的函数原型为： re.search(pattern, string, flags)

每个参数的含意与re.match一样。 

---

## re.match与re.search的区别

re.match只匹配字符串的开始，如果字符串从一开始就不符合正则表达式，则匹配失败，函数返回None；而re.search匹配整个字符串，直到找到一个匹配。

请看下面这个实例：


```
import re
line = "This is a very beautiful girl, I like her very much.";
m = re.match( r'girl', line, re.M|re.I)
if m:
   print "match --> m.group() : ", m.group()
else:
   print "No match!!"
```
<pre><code class="markdown">
search --> m.group() :  girl
search --> matchObj.group() :  dogs
</code></pre>

```
m = re.search( r'girl', line, re.M|re.I)
if m:
   print "search --> m.group() : ", m.group()
else:
   print "No match!"
```

以上实例运行结果如下：


<pre><code class="markdown">
search --> m.group() :  girl
</code></pre>

---

## re.sub函数

re.sub用于替换字符串中的匹配项。下面一个例子将字符串中的空格 ' ' 替换成 '-' : 

```
import re
text = "I like Cats more than dogs!"
print re.sub(r'\s+', '-', text) 

```

输出：

<pre><code class="markdown">
I-like-Cats-more-than-dogs!
</code></pre>


re.sub的函数原型为：re.sub(pattern, repl, string, count)

其中第二个函数是替换后的字符串；本例中为'-'

第四个参数指替换个数。默认为0，表示每个匹配项都替换。

re.sub还允许使用函数对匹配项的替换进行复杂的处理。如：re.sub(r'\s', lambda m: '[' + m.group(0) + ']', text, 0)；将字符串中的空格' '替换为'[ ]'。


---


## re.split函数

可以使用re.split来分割字符串，如：re.split(r'-', text)；将字符串按'-'符号分割成一个单词列表。

```
import re
text="I-really-like-this-girl!"
re.split(r'-',text)
```

输出：

<pre><code class="markdown">
['I', 'really', 'like', 'this', 'girl!']
</code></pre>


---

## re.findall函数

re.findall可以获取字符串中所有匹配的字符串。如：re.findall(r'\w*i\w*', text)；获取字符串中，包含'oo'的所有单词。


```
import re
text="I-really-like-this-girl!"
re.findall(r'girl',text)
```
输出结果：
<pre><code class="markdown">
['like', 'this', 'girl']
</code></pre>

---

## re.compile函数

可以把正则表达式编译成一个正则表达式对象。可以把那些经常使用的正则表达式编译成正则表达式对象，这样可以提高一定的效率。下面是一个正则表达式对象的一个例子：


```
import re
regex = re.compile(r'\w*er\w*') # 将正则表达式编译成Pattern对象
text = "This is a very beautiful girl, I like her very much."
m = regex.search(text) #使用regex来匹配text字符串
if m:
	print m.group() # 使用Match获得分组信息
print regex.findall(text)   #查找所有包含'oo'的单词
print regex.sub(lambda m: '[' + m.group(0) + ']', text) #将字符串中含有'oo'的单词用[]括起来。
```

分别输出下列信息：

<pre><code class="markdown">
'very'
['very', 'her', 'very']
This is a [very] beautiful girl, I like [her] [very] much.
</code></pre>

---

## 邮箱验证

使用Python写一个简单的邮箱验证的正则表达式：

根据csu.ldw@csu.edu.cn来填写规则

规则：

- @前面可以有'.'、‘_’,'-'，但不能出现在头尾，而且不能连续出现
- @后面到结尾之间，可以有多个子域名
- 邮箱的结尾为2~5个字母，比如cn、com、name等
 
```
#-*- coding: utf-8 -*-
"""
Created on Thu Oct 29 20:28:57 2015
@author: liudiwei
"""
import re
regex = re.compile('^[A-Za-z0-9]+(([\.\_\-])?[A-Za-z0-9]+)+@([A-Za-z]+.)+[A-Za-z]{2,5}$')
m = regex.match("csu.ldw@csu.edu.cn")
if m:
    print m.group()
else:
    print "no match!"
```

测试输出：

<pre><code class="markdown">
csu.ldw@csu.edu.cn
</code></pre>

当m = regex.match("_csu.ldw@csu.edu.cn")
当邮箱为：

<pre><code class="markdown">
_csu.ldw@csu.edu.cn  
csu.ldw_@csu.edu.cn
csu.ldw@csu_.edu.cn
_csu.ldw@csu.edu.cn1
</code></pre>

都不会匹配

提示：合法邮箱的规则可能不够完善，这里就简单的匹配这三个规则吧！

---