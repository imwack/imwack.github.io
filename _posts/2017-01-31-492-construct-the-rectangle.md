---
layout: post
title: "492. Construct the Rectangle"
date: 2017-01-31 15:49
author: imwack
comments: true
categories: [LeetCode]
tags: []
---
给定矩形面积，要求输出宽W和长L，并且W和L之差最小

**Example:**


**Input:** 4
    **Output:** [2, 2]
    **Explanation:** The target area is 4, and all the possible ways to construct it are [1,4], [2,2], [4,1].</pre>
    <pre class="pure-highlightjs"><code class="">    vector&lt;int&gt; constructRectangle(int area) {
            vector&lt;int&gt; ret;
            int L = sqrt(area);
            while(area%L!=0){
                L--;
            }
            int W = area/L;
            ret.push_back(W);
            ret.push_back(L);
            return ret;
        }`

&nbsp;
