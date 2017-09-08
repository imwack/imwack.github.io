---
layout: post
title: "STL基本算法 algobase - 1 概述"
date: 2017-02-24 14:34
author: imwack
comments: true
categories: [STL]
tags: []
---
STL常用的算法定义于&lt;stl_algobase.h&gt;中其他算法定义于&lt;stl_algo.h&gt;中，下面是一些常用的函数使用方法，具体的解析后文在做介绍


    #include&lt;algorithm&gt;
    #include&lt;vector&gt;
    #include&lt;functional&gt;
    #include&lt;iostream&gt;
    #include&lt;iterator&gt;
    #include&lt;string&gt;
    using namespace std;
    
    //仿函数用于输出
    template &lt;class T&gt;
    struct display{
        void operator()(const T&amp; x) const
        {
            cout&lt;&lt;x&lt;&lt;' '&lt;&lt;;
        }
    };
    
    
    int _tmain(int argc, _TCHAR* argv[])
    {
        int ia[9] = {0,1,2,3,4,5,6,7,8};
        vector&lt;int&gt; iv1(ia,ia+5);    //{0,1,2,3,4}
        vector&lt;int&gt; iv2(ia,ia+9);    //{0,1,2,3,4,5,6,7,8}
    
        //mismatch判断两个区间第一个不匹配点 返回两个迭代器组成的pair
        //这里会报错 因为iv1的迭代器指向了end()
        //cout&lt;&lt;*(mismatch(iv1.begin(),iv1.end(),iv2.begin()).first) ;    
        cout&lt;&lt;*(mismatch(iv1.begin(),iv1.end(),iv2.begin()).second);    //输出5
    
        //equal 比较区间内元素是否相同 若第二个迭代器较长也无所谓
        cout&lt;&lt;equal(iv1.begin(),iv1.end(),iv2.begin());        //输出1
    
        fill(iv1.begin(),iv1.end(),9);    //区间内全部赋值9
        fill_n(iv1.begin(),3,7);        //从迭代器开始位置填3个7
    
        auto it1 = iv1.begin();
        auto it2 = iv1.begin();
        advance(it1,3);        //相当于it+3
        cout&lt;&lt;*it1;            //输出9
    
        iter_swap(it1,it2);    //交换两个迭代器所指元素
        swap(*iv1.begin(),*iv2.begin());    //交换两个数值
    
        return 0;
    }
    
    `

&nbsp;

&nbsp;
