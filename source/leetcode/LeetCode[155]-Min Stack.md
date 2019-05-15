---
title: LeetCode[155]-Min Stack
layout: page
noDate: "true"
comments: true
description: "LeetCode题解" 
---
<article class="post post-type-normal" itemscope="" itemtype="http://schema.org/Article" style="opacity: 1; transform: translateY(0px);">

Link: https://leetcode.com/problems/min-stack/

Design a stack that supports push, pop, top, and retrieving the minimum element in constant time.

push(x) -- Push element x onto stack.
pop() -- Removes the element on top of the stack.
top() -- Get the top element.
getMin() -- Retrieve the minimum element in the stack.

------
思路：需要两个栈，一个用来作为一般的栈，另一个用来存放进栈到某一位置的当前站内最小元素，代码如下：

Code（c++）:


```
class MinStack {
public:
    std::stack<int> stk;
    std::stack<int> min;
    void push(int x) {
        stk.push(x);
        if(min.empty() || (!min.empty() && x <= min.top())){
            min.push(x);
        }
    }

    void pop() {
        if(!stk.empty()){
            if(stk.top() == min.top())min.pop();
            stk.pop();
        }
    }

    int top() {
        if(!stk.empty())
            return stk.top();
    }

    int getMin() {
        if(!stk.empty()) return min.top();
    }
};
```


</article>
