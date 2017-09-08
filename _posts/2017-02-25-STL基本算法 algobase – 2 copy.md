---
layout: post
title: "STL基本算法 algobase – 2 copy"
date: 2017-02-25 10:31
author: imwack
comments: true
categories: [STL]
tags: []
---
由于普通算法的实现非常简单，就不在继续讨论其具体实现，而copy算法是STL里常使用的函数，其复制操作效率极高，因为他使用了各种重载、偏特化等技巧。

copy是将[first,last)区间的元素复制到[result, result+(last-first)) 输入区间由InputIterator 输出区间为OutputIterator

1.两个特化版本，针对原生指针const char*和const wchar_t *直接使用内存拷贝


     #define __SGI_STL_DECLARE_COPY_TRIVIAL(_Tp)                                \
      inline _Tp* copy(const _Tp* __first, const _Tp* __last, _Tp* __result) { \
        memmove(__result, __first, sizeof(_Tp) * (__last - __first));          \
        return __result + (__last - __first);                                  \
      }`</pre>
    这里和书上有出入，这里包括了所有const的trivial类型([trivial参考另一篇文章](http://www.imwack.cn/index.php/2017/02/25/422/))见下表
    <pre class="pure-highlightjs"><code class="">__SGI_STL_DECLARE_COPY_TRIVIAL(char)
    __SGI_STL_DECLARE_COPY_TRIVIAL(signed char)
    __SGI_STL_DECLARE_COPY_TRIVIAL(unsigned char)
    __SGI_STL_DECLARE_COPY_TRIVIAL(short)
    __SGI_STL_DECLARE_COPY_TRIVIAL(unsigned short)
    __SGI_STL_DECLARE_COPY_TRIVIAL(int)
    __SGI_STL_DECLARE_COPY_TRIVIAL(unsigned int)
    __SGI_STL_DECLARE_COPY_TRIVIAL(long)
    __SGI_STL_DECLARE_COPY_TRIVIAL(unsigned long)
    #ifdef __STL_HAS_WCHAR_T
    __SGI_STL_DECLARE_COPY_TRIVIAL(wchar_t)
    #endif
    #ifdef _STL_LONG_LONG
    __SGI_STL_DECLARE_COPY_TRIVIAL(long long)
    __SGI_STL_DECLARE_COPY_TRIVIAL(unsigned long long)
    #endif 
    __SGI_STL_DECLARE_COPY_TRIVIAL(float)
    __SGI_STL_DECLARE_COPY_TRIVIAL(double)
    __SGI_STL_DECLARE_COPY_TRIVIAL(long double)
    
    #undef __SGI_STL_DECLARE_COPY_TRIVIAL
    #endif /* __STL_CLASS_PARTIAL_SPECIALIZATION */`</pre>
    2.copy的泛化版本,调用__copy_dispatch
    <pre class="pure-highlightjs"><code class="">template &lt;class _InputIter, class _OutputIter&gt;
    inline _OutputIter copy(_InputIter __first, _InputIter __last,
                            _OutputIter __result) {
      __STL_REQUIRES(_InputIter, _InputIterator);
      __STL_REQUIRES(_OutputIter, _OutputIterator);
      typedef typename iterator_traits&lt;_InputIter&gt;::value_type _Tp;
      typedef typename __type_traits&lt;_Tp&gt;::has_trivial_assignment_operator
              _Trivial;
      return __copy_dispatch&lt;_InputIter, _OutputIter, _Trivial&gt;
        ::copy(__first, __last, __result);
    }`</pre>
    3.__copy_dispatch有两个偏特化版本和一个泛化版本，模板参数_Trivial指定了类型是否为trivial类型
    
    3.1先看偏特化版本，这里需要指针所指之物为trivial也就是代码中的true_type，trivial直接用memmove效率最高
    <pre class="pure-highlightjs"><code class="">template &lt;class _Tp&gt;  //两个参数都是T*
    struct __copy_dispatch&lt;**_Tp*, _Tp*,** **__true_type**&gt;
    {
      static _Tp* copy(const _Tp* __first, const _Tp* __last, _Tp* __result) {
        return __copy_trivial(__first, __last, __result);
      }
    };
    
    template &lt;class _Tp&gt; //一个const T* 一个T*
    struct __copy_dispatch&lt;**const _Tp*, _Tp***, **__true_type**&gt;
    {
      static _Tp* copy(const _Tp* __first, const _Tp* __last, _Tp* __result) {
        return __copy_trivial(__first, __last, __result);
      }
    };`</pre>
    trivial类型的调用__copy_trivial直接memmove
    <pre class="pure-highlightjs"><code class="">template &lt;class _Tp&gt;
    inline _Tp*
    __copy_trivial(const _Tp* __first, const _Tp* __last, _Tp* __result) {
      **memmove**(__result, __first, sizeof(_Tp) * (__last - __first));
      return __result + (__last - __first);
    }`</pre>
    &nbsp;
    
    3.3完全泛化版本，这里萃取出迭代器种类，再调用__copy
    <pre class="pure-highlightjs"><code class="">template &lt;class _InputIter, class _OutputIter, class _BoolType&gt;
    struct __copy_dispatch {
      static _OutputIter copy(_InputIter __first, _InputIter __last,
                              _OutputIter __result) {
        typedef typename iterator_traits&lt;_InputIter&gt;::**iterator_category** _Category;
        typedef typename iterator_traits&lt;_InputIter&gt;::difference_type _Distance;
        return __copy(__first, __last, __result, _Category(), (_Distance*) 0);
      }
    };`</pre>
    根据迭代器种类提供不同的__copy版本(Input/Random Access迭代器)
    <pre class="pure-highlightjs"><code class="">template &lt;class _InputIter, class _OutputIter, class _Distance&gt;
    inline _OutputIter __copy(_InputIter __first, _InputIter __last,
                              _OutputIter __result,
                              **input_iterator_tag**, _Distance*)
    {                         **//这里是针对input iterator**
      for ( ; __first != __last; ++__result, ++__first)
        *__result = *__first;
      return __result;
    }
    
    template &lt;class _RandomAccessIter, class _OutputIter, class _Distance&gt;
    inline _OutputIter
    __copy(_RandomAccessIter __first, _RandomAccessIter __last,
           _OutputIter __result, **random_access_iterator_tag**, _Distance*)
    {                          //针对RAI  
      for (**_Distance** __n = __last - __first; __n &gt; 0; --__n) {
        *__result = *__first;
        ++__first;
        ++__result;
      }  **//书上说这里用n决定循环次数 速度更快**
      return __result;
    }`

&nbsp;
