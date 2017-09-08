---
layout: post
title: "476. Number Complement"
date: 2017-01-30 23:49
author: imwack
comments: true
categories: [LeetCode]
tags: []
---
Given a positive integer, output its complement number. The complement strategy is to flip the bits of its binary representation. 翻转一个数的二进制

**Example 1:**


**Input:** 5 （101）
    **Output:** 2（010）</pre>
    <pre class="pure-highlightjs"><code class="">    int findComplement(int num) {
            int mask = ~0;    
            while(num&amp;mask)
                mask&lt;&lt;=1;
            return ~mask &amp; ~num;
        }`

&nbsp;
