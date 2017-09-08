---
layout: post
title: "453. Minimum Moves to Equal Array Elements"
date: 2017-02-01 10:49
author: imwack
comments: true
categories: [LeetCode]
tags: []
---
Given a **non-empty** integer array of size *n*, find the minimum number of moves required to make all array elements equal, where a move is incrementing *n* - 1 elements by 1.

给定数组，求出移动数组元素最少次数，使得数组中元素相等，每次移动的规则是将n-1个数+1（n为数组大小）

**Example:**


**Input:**
    [1,2,3]
    
    **Output:**
    3
    
    **Explanation:**
    Only three moves are needed (remember each move increments two elements):
    
    [1,2,3]  =&gt;  [2,3,3]  =&gt;  [3,4,3]  =&gt;  [4,4,4]</pre>
    
    <hr />
    
    由  sum(array)+m(n-1) = x*n      m为移动次数 x为相等的和
    
    x = min(array)+m
    
    m = sum(array) - min(array)*n       参考网上的答案
    <pre class="pure-highlightjs"><code class="">    int minMoves(vector&lt;int&gt;&amp; nums) {
            int sum = 0;
            if(nums.size()&lt;=0)
                return 0;
            int min = nums[0];
            for(int i=0;i&lt;nums.size();i++){
                if(nums[i]&lt;min)
                    min = nums[i];
                sum+=nums[i];
            }
            return sum-min*nums.size();
        }`

&nbsp;
