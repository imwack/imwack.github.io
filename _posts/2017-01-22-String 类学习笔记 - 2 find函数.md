---
layout: post
title: "String 类学习笔记 - 2 find函数"
date: 2017-01-22 21:42
author: imwack
comments: true
categories: [C++, string]
tags: []
---
字符串的一些查找函数，find较为常用

 
        //从pos位置开始查找字串str，找到返回位置，没找到返回string::npos
        size_t find (const string&amp; str, size_t pos = 0) const;
        size_t find (const char* s, size_t pos = 0) const;
        //查找str位置pos开始的n个字串
        size_t find (const char* s, size_t pos, size_t n) const;    
        //查找字符c出现的首个位置
        size_t find (char c, size_t pos = 0) const
    
        //其他查找函数
        rfind                //查找字串或字符出现的最后一次位置
        find_first_of        //查找给定串中第一个出现的字符位置
        find_last_of        //查找给定串中最后一个出现的字符位置
        find_first_not_of    //查找第一个不包含在参数中的字符    
        find_last_not_of    //查找最后一个不包含在参数中的字符`

&nbsp;
