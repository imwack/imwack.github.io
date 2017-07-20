---
layout: post
title: "30.	最小的K个数*"
date: 2017-01-21 14:10
author: imwack
comments: true
categories: [剑指offer]
tags: []
---
<h2 class="subject-item-title">题目描述


<div class="subject-describe">输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。</div>
		使用排序 O(n logn) 
        vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
             vector<int> ret;
            if(input.size()<k)
                return ret;
            sort(input.begin(),input.end());
            for(int i=0;i<k;i++)
                ret.push_back(input[i]);
            return ret;
        }