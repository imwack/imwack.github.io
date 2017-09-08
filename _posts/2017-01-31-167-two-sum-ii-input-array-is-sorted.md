---
layout: post
title: "167. Two Sum II - Input array is sorted"
date: 2017-01-31 16:05
author: imwack
comments: true
categories: [LeetCode]
tags: []
---
给定升序数组、目标和target，求出和为目标和的两个数的下标index1,index2

**Input:** numbers={2, 7, 11, 15}, target=9
**Output:** index1=1, index2=2

My Solution


	   vector&lt;int&gt; twoSum(vector&lt;int&gt;&amp; numbers, int target) {
            vector&lt;int&gt; ret;
            int right = numbers.size()-1;
            int left = 0;
            while(left&lt;right &amp;&amp; numbers[left]+numbers[right]!=target){
                if(numbers[left]+numbers[right]&gt;target)
                    right--;
                else
                    left++;
            }
            if(numbers[left]+numbers[right]==target){
                ret.push_back(left+1);
                ret.push_back(right+1);
            }
            return ret;
        }`

实际上我这里没考虑不存在的情况 应该考虑一下
