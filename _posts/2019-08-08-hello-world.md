---
layout: post
title: 'Hello World'
subtitle:   " \"Hello World, Hello Blog\""
date: 2019-08-08
author: Max.C
header-img: 'assets/img/pro1.png'
catalog: true
tags: C++ test
---

# Welcome  

作为测试的第一篇博客，惯例Hello World，顺便熟悉一下`markdown`语法。

``` cpp

#include<iostream>
using namespace std;

int main(){
	cout<<"hello world";
	return 0;
}

```

```flow

st=>start: 开始
op=>operation: My Operation
cond=>condition: Yes or No?
e=>end
st->op->cond
cond(yes)->e
cond(no)->op

```

```mermaid

graph LR
    start[开始] --> input[输入A,B,C]
    input --> conditionA{A是否大于B}
    conditionA -- YES --> conditionC{A是否大于C}
    conditionA -- NO --> conditionB{B是否大于C}
    conditionC -- YES --> printA[输出A]
    conditionC -- NO --> printC[输出C]
    conditionB -- YES --> printB[输出B]
    conditionB -- NO --> printC[输出C]
    printA --> stop[结束]
    printC --> stop
    printB --> stop
    
```

***