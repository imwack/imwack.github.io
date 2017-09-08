---
layout: post
title: "数值算法numeric初探 - 4 inner_product"
date: 2017-02-24 10:55
author: imwack
comments: true
categories: [STL]
tags: []
---
inner_product也提供两个原型

    template &lt;class _InputIterator1, class _InputIterator2, class _Tp&gt;
    _Tp inner_product(_InputIterator1 __first1, _InputIterator1 __last1,
                      _InputIterator2 __first2, _Tp __init)
    {
      __STL_REQUIRES(_InputIterator2, _InputIterator);
      __STL_REQUIRES(_InputIterator2, _InputIterator);
      for ( ; __first1 != __last1; ++__first1, ++__first2)
        __init = __init + (*__first1 * *__first2);  
      return __init;
    }
    
    template &lt;class _InputIterator1, class _InputIterator2, class _Tp,
              class _BinaryOperation1, class _BinaryOperation2&gt;
    _Tp inner_product(_InputIterator1 __first1, _InputIterator1 __last1,
                      _InputIterator2 __first2, _Tp __init, 
                      _BinaryOperation1 __binary_op1,
                      _BinaryOperation2 __binary_op2)
    {
      __STL_REQUIRES(_InputIterator2, _InputIterator);
      __STL_REQUIRES(_InputIterator2, _InputIterator);
      for ( ; __first1 != __last1; ++__first1, ++__first2)
        __init = __binary_op1(__init, __binary_op2(*__first1, *__first2));
      return __init;
    }`</pre>
    这个函数原型非常简单，版本1只是计算init + 迭代器1的值* 迭代器2的值
    
    版本2需要提供两个二元操作符，计算如下
    <pre class="pure-highlightjs"><code class="">__init = __binary_op1(__init, __binary_op2(*__first1, *__first2));`

&nbsp;
