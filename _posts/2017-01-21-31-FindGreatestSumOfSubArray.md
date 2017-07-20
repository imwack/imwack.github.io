---
layout: post
title: "31.连续子数组的最大和"
date: 2017-01-21 14:12
author: imwack
comments: true
categories: [剑指offer]
tags: []
---
<h2 class="subject-item-title">题目描述


<div class="subject-describe">例如:{6,-3,-2,7,-15,1,2,2},连续子向量的最大和为8(从第0个开始,到第3个为止)。你会不会被他忽悠住？(子向量的长度至少是1)</div>
<div class="subject-describe"></div>
<div class="subject-describe">实际上是动态规划</div>
<div class="subject-describe">Fn = Fn-1 + An (Fn-1 >0)</div>
<div class="subject-describe">Fn = An                 (Fn-1 <0)</div>
<div class="subject-describe">

	int FindGreatestSumOfSubArray(vector<int> array) {
            if(array.size()<=0)
                return 0;
            int maxF = array[0];
            int Fi = array[0];    //F0
            for(int i=1;i<array.size();i++){
                if(Fi<0)
                    Fi = array[i];
                else
                    Fi = array[i]+Fi;
                if(Fi>maxF)
                    maxF = Fi;
            }
            return maxF;
        }
