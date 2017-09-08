---
layout: post
title: "数值算法numeric初探 - 5 partial_sum"
date: 2017-02-24 10:56
author: imwack
comments: true
categories: [STL]
tags: []
---
partial_sum意为部分和，类似的提供两个版本


 
    template &lt;class _InputIterator, class _OutputIterator, class _Tp&gt;
    _OutputIterator 
    __partial_sum(_InputIterator __first, _InputIterator __last,
                  _OutputIterator __result, _Tp*)
    {
      _Tp __value = *__first;
      while (++__first != __last) {
        **__value = __value + *__first;  //累加当前元素和之前的和**
        *++__result = __value;
      }
      return ++__result;
    }
    
    template &lt;class _InputIterator, class _OutputIterator&gt;
    _OutputIterator 
    partial_sum(_InputIterator __first, _InputIterator __last,
                _OutputIterator __result)
    {
      __STL_REQUIRES(_InputIterator, _InputIterator);
      __STL_REQUIRES(_OutputIterator, _OutputIterator);
      if (__first == __last) return __result;
     ** *__result = *__first;       //第一个元素为自己**
      return __partial_sum(__first, __last, __result, __VALUE_TYPE(__first));
    }`</pre>
    版本二额外提供了指定二元运算符
    <pre class="pure-highlightjs"><code class="">
    template &lt;class _InputIterator, class _OutputIterator, class _Tp,
              class _BinaryOperation&gt;
    _OutputIterator 
    __partial_sum(_InputIterator __first, _InputIterator __last, 
                  _OutputIterator __result, _Tp*, _BinaryOperation __binary_op)
    {
      _Tp __value = *__first;
      while (++__first != __last) {
        **__value = __binary_op(__value, *__first);  //仅在此处做了修改**
        *++__result = __value;
      }
      return ++__result;
    }
    
    template &lt;class _InputIterator, class _OutputIterator, class _BinaryOperation&gt;
    _OutputIterator 
    partial_sum(_InputIterator __first, _InputIterator __last,
                _OutputIterator __result, _BinaryOperation __binary_op)
    {
      __STL_REQUIRES(_InputIterator, _InputIterator);
      __STL_REQUIRES(_OutputIterator, _OutputIterator);
      if (__first == __last) return __result;
      ***__result = *__first;**
      return __partial_sum(__first, __last, __result, __VALUE_TYPE(__first), 
                           __binary_op);
    }
    `

numerical 数值计算库还是比较好理解的，这一块比较简单。
