---
layout: post
title: "448. Find All Numbers Disappeared in an Array"
date: 2017-01-31 15:26
author: imwack
comments: true
categories: [LeetCode]
tags: []
---
Given an array of integers where 1 ≤ a[i] ≤ *n* (*n* = size of array), some elements appear twice and others appear once.

Find all the elements of [1, *n*] inclusive that do not appear in this array.

Could you do it without extra space and in O(*n*) runtime? You may assume the returned list does not count as extra space.

**Example:**


**Input:**
    [4,3,2,7,8,2,3,1]
    
    **Output:**
    [5,6]</pre>
    My Solution
    <pre class="pure-highlightjs"><code class="">    vector&lt;int&gt; findDisappearedNumbers(vector&lt;int&gt;&amp; nums) {
            bool *flag = new bool[nums.size()];
            vector&lt;int&gt; ret;
            memset(flag,0,nums.size());
            for(int i=0;i&lt;nums.size();i++){
                if(!flag[nums[i]-1])
                    flag[nums[i]-1] = true;
            }
            for(int i=0;i&lt;nums.size();i++){
                if(!flag[i])
                    ret.push_back(i+1);
            }
            return ret;
        }
    `</pre>
    我用了O（n）的额外空间，网上的解答更加简单
    <pre class="pure-highlightjs"><code class="">    vector&lt;int&gt; findDisappearedNumbers(vector&lt;int&gt;&amp; nums) {
    
            vector&lt;int&gt; ret;
            int len = nums.size();
            for(int i=0;i&lt;len;i++){
                int val = abs(nums[i])-1;
                if(nums[val]&gt;0)
                    nums[val] = -nums[val];
            }
            for(int i=0;i&lt;len;i++){
                if(nums[i]&gt;0)
                ret.push_back(i+1);
            }
            return ret;
        }`

&nbsp;
