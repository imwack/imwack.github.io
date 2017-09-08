---
layout: post
title: "STL list::sort() 学习"
date: 2017-02-17 16:07
author: imwack
comments: true
categories: [STL]
tags: []
---
前段时间一直感冒发烧，耽误了学习进度，今天开始又继续学习STL，今天看到链表list中的一个sort()算法，有点蒙B，书上并没有细说，故自己深入进行了一番研究。

首先需要了解几个函数


	1.splice的一个重载，将迭代器所指位置的元素移动到pos处
      void splice(iterator __position, list&amp;, iterator __i) {
        iterator __j = __i;
        ++__j;
        if (__position == __i || __position == __j) return;
        this-&gt;transfer(__position, __i, __j);
      }`</pre>
    <pre class="pure-highlightjs"><code class="">2.merge函数，将排序好的链表x合并入原链表
    template &lt;class _Tp, class _Alloc&gt;
    void list&lt;_Tp, _Alloc&gt;::merge(list&lt;_Tp, _Alloc&gt;&amp; __x)
    {
      iterator __first1 = begin();
      iterator __last1 = end();
      iterator __first2 = __x.begin();
      iterator __last2 = __x.end();
      while (__first1 != __last1 &amp;&amp; __first2 != __last2)
        if (*__first2 &lt; *__first1) {
          iterator __next = __first2;
          transfer(__first1, __first2, ++__next);
          __first2 = __next;
        }
        else
          ++__first1;
      if (__first2 != __last2) transfer(__last1, __first2, __last2);
    }`</pre>
    splice merge都使用了transfer函数(书上有分析) 其复杂度是O(1)的，merge复杂度即为O(N)
    
    因为这两个函数在sort中出现故先说明，下面是sort函数的源码
    <pre class="pure-highlightjs"><code class="">template &lt;class _Tp, class _Alloc&gt;
    void list&lt;_Tp, _Alloc&gt;::sort()
    {
      //长度0或1则不需要处理
      if (_M_node-&gt;_M_next != _M_node &amp;&amp; _M_node-&gt;_M_next-&gt;_M_next != _M_node) {
        list&lt;_Tp, _Alloc&gt; __carry;
        list&lt;_Tp, _Alloc&gt; __counter[64];
        int __fill = 0;
        while (!empty()) {
          __carry.splice(__carry.begin(), *this, begin());  //每次取链表的开头元素
          int __i = 0;
          while(__i &lt; __fill &amp;&amp; !__counter[__i].empty()) {
            __counter[__i].merge(__carry);
            __carry.swap(__counter[__i++]);
          }
          __carry.swap(__counter[__i]);         
          if (__i == __fill) ++__fill;
        } 
    
        for (int __i = 1; __i &lt; __fill; ++__i)
          __counter[__i].merge(__counter[__i-1]);
        swap(__counter[__fill-1]);
      }
    }`</pre>
    书上写的是这里用的快排，实际上却是归并排序，并且方法很独特。
    
    数组counter[64]用于存放中间变量，counter[i]可以存放2^(i+1)个元素，即2,4,8,16...2^64，最多2的64次方个元素，用于归并排序。
    
    例如list 为 9 8 7 6 5 4 3 2 1.
    

1.  carry取出元素9，i=0,fill=0, counter[0] = 9; fill =1
2.  carry取8,此时进入while循环，counter[0] merge为[8,9] carry和counter[0]交换            退出循环counter[1]为[8,9] ，counter[0]为空 fill=2
3.  carry取7，counter[0]为空不进入while，交换counter[0]为7
4.  carry取6，counter[1] merge[6,7] ，i=1,再merge counter[2] = [6,7,8,9]
5.  5/X/6789       //斜杠分割表示counter 0,1,...   X表示元素为空
6.  X/45/6789
7.  3/45/6789
8.  X/X/X/23456789
9.  1/X/X/23456789
10.  此时进入最后一个for循环 将所有元素merge
    测试代码
    <pre class="pure-highlightjs"><code class="">void printList(list&lt;int&gt; l){
        list&lt;int&gt;::iterator it = l.begin();
        while (it != l.end()){
            cout &lt;&lt; *it &lt;&lt; " ";
            it++;
        }
        cout &lt;&lt; endl;
    }
    int _tmain(int argc, _TCHAR* argv[])
    {
        list&lt;int&gt; l;
        for (int i = 9; i &gt;= 0;i--)
            l.push_back(i);
        list&lt;int&gt; carry;
        list&lt;int&gt; counter[64];
        int fill = 0;
        while (!l.empty()){
            carry.splice(carry.begin(), l, l.begin());
            int i = 0;
            while (i&lt;fill &amp;&amp; !counter[i].empty())
            {
                counter[i].merge(carry);
                carry.swap(counter[i++]);
            }
            cout &lt;&lt; "carry: "; printList(carry);
            carry.swap(counter[i]);
            cout &lt;&lt; "counter[" &lt;&lt; i&lt;&lt;"]: " ; printList(counter[i]);
            if (i == fill)fill++;
        }
        for (int i = 1; i &lt; fill; i++)
            counter[i].merge(counter[i - 1]);
    
        return 0;
    }`

实际上整个过程非常复杂，弄完了也是糊里糊涂的，归并排序复杂度O(n logn)，大牛的实现实在是常人难以理解。。
