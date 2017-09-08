---
layout: post
title: "数值算法numeric初探 - 3 adjacent_difference"
date: 2017-02-24 10:35
author: imwack
comments: true
categories: [STL]
tags: []
---
adjacent意外相邻的，顾名思义相邻元素的差别，首先是使用方法


      adjacent_difference(iv.begin(),iv.end(),oit);
        //v[0] v[1]-v[0] v[2]-v[1]  当前项减前一项
        adjacent_difference(iv.begin(),iv.end(),oit,plus&lt;int&gt;());
        //v[0] v[1]+v[0] v[2]+v[1]  op(当前项，前一项)`</pre>
    下面看一下STL的版本1 只有迭代器的版本的
    <pre class="pure-highlightjs"><code class="">template &lt;class _InputIterator, class _OutputIterator&gt;
    _OutputIterator
    adjacent_difference(_InputIterator __first,
                        _InputIterator __last, _OutputIterator __result)
    {
      __STL_REQUIRES(_InputIterator, _InputIterator);
      __STL_REQUIRES(_OutputIterator, _OutputIterator);
      if (__first == __last) return __result;   //空区间
      ***__result = *__first;                    //记录第一个元素**
      **return __adjacent_difference(__first, __last, __result,
                                   __VALUE_TYPE(__first));**
    }
    //由`<code class="">adjacent_difference 调用萃取出value_type
    `<code class="">template &lt;class _InputIterator, class _OutputIterator, class _Tp&gt;
    _OutputIterator 
    __adjacent_difference(_InputIterator __first, _InputIterator __last,
     _OutputIterator __result, _Tp*)
    {   //这里不用考虑空区间了
      _Tp __value = *__first;
      while (++__first != __last) {  //遍历区间
      _Tp __tmp = *__first;
    **  *++__result = __tmp - __value;  //计算当前项减前一项的差，赋值给**`**<code class="">OutputIterator`**<code class="">
      __value = __tmp;
      }
      return ++__result;
    }
    
    `</pre>
    版本二 带_BinaryOperation二元操作符的
    <pre class="pure-highlightjs"><code class="">template &lt;class _InputIterator, class _OutputIterator, class _BinaryOperation&gt;
    _OutputIterator 
    adjacent_difference(_InputIterator __first, _InputIterator __last,
                        _OutputIterator __result, _BinaryOperation __binary_op)
    {
      __STL_REQUIRES(_InputIterator, _InputIterator);
      __STL_REQUIRES(_OutputIterator, _OutputIterator);
      if (__first == __last) return __result; //空区间
    **  *__result = *__first;          // 记录第一个元素并调用**`**<code class="">__adjacent_difference
    `**<code class="">**  return __adjacent_difference(__first, __last, __result, __VALUE_TYPE(__first), __binary_op);** 
    }
    template &lt;class _InputIterator, class _OutputIterator, class _Tp, 
     class _BinaryOperation&gt;
    _OutputIterator
    __adjacent_difference(_InputIterator __first, _InputIterator __last, 
     _OutputIterator __result, _Tp*,
     _BinaryOperation __binary_op) {
     _Tp __value = *__first;
     while (++__first != __last) {
     _Tp __tmp = *__first;
     ***++__result = __binary_op(__tmp, __value); //只是此处修改了二元运算符**
     __value = __tmp;
     }
     return ++__result;
    }
    `

如果令result等于first那么这里就是质变的算法，会改变原来的元素。
