---
layout: post
title: "C++中的trivial、POD的概念"
date: 2017-02-25 10:40
author: imwack
comments: true
categories: [C++]
tags: []
---
在看STL源码的时候，经常看到trivial、POD等名词，今天也是学习整理了一下

1.trivial翻译为无意义的，trivial和non-trivial针对类的以下函数：


*   构造函数(ctor)
*   复制构造函数(copy)
*   赋值函数(assignment)
*   析构函数(dtor)
如果至少满足下面3条里的一条，则为non-trivial：


1.  显式(explict)定义了这四种函数。
2.  类里有非静态非POD的数据成员。
3.  有基类。
（参考[http://blog.csdn.net/a627088424/article/details/48595525](http://blog.csdn.net/a627088424/article/details/48595525)）

也就是说如果没有定义ctor\dtor\copy\assignment一般都是trivial的。

2.POD意思是Plain Old Data，也就是C++的内建类型或传统的C结构体类型。

POD类型必然有trivial ctor/dtor/copy/assignment四种函数。

3.如果类是trivial的那么它是无意义的，也就是他没有ctor等函数，那么在copy的时候就可以直接用底层的内存操作，提高效率。
