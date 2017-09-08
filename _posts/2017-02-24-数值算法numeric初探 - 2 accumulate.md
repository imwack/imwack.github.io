---
layout: post
title: "数值算法numeric初探 - 2 accumulate"
date: 2017-02-24 10:15
author: imwack
comments: true
categories: [STL]
tags: []
---
实现是accumulate元素累计函数的实现，英文原意为累计，下面是STL源码


    //版本1 单纯累加
    template &lt;class _InputIterator, class _Tp&gt;
    _Tp accumulate(_InputIterator __first, _InputIterator __last, _Tp __init)
    {
      __STL_REQUIRES(_InputIterator, _InputIterator);
      for ( ; __first != __last; ++__first)
        __init = __init + *__first;  //元素累加到init上
      return __init;
    }
    //版本2 可传入functor
    template &lt;class _InputIterator, class _Tp, class _BinaryOperation&gt;
    _Tp accumulate(_InputIterator __first, _InputIterator __last, _Tp __init,
                   _BinaryOperation __binary_op)
    {
      __STL_REQUIRES(_InputIterator, _InputIterator);
      for ( ; __first != __last; ++__first)
        __init = __binary_op(__init, *__first);  //根据二元运算符计算结果
      return __init;
    }`

accumulate的实现较为简单易懂
