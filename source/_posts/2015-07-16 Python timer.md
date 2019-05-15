---
layout: post
title: "一个简单的Python函数运行时间计时器"
date: 2015-07-16 20:24:25
categories: Python
tag: 
	- 计时器
	- Python
---


在实际开发中，往往想要计算一段代码运行多长时间，下面我将该功能写入到一个函数里面，只要在每个函数前面调用该函数即可，见下面代码：

<!--more-->

```python
#--------------------------------
import time
from functools import wraps  
def fun_timer(function):
    @wraps(function)
    def function_timer(*args, **kwargs):
        t0 = time.time()
        result = function(*args, **kwargs)
        t1 = time.time()
        os.system(" echo Total time running %s: %s seconds" % (function.func_name, str(t1-t0)) + " >> timecount.log")
        return result
    return function_timer
#-----------------------------------
```

说明：<font color="green">**一个记时器，只要在函数前面写上@fun_timer即可**</font>.


---