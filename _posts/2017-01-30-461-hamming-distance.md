---
layout: post
title: "461. Hamming Distance"
date: 2017-01-30 23:46
author: imwack
comments: true
categories: [LeetCode]
tags: []
---
**Example:**


**Input:** x = 1, y = 4
    
    **Output:** 2
    
    **Explanation:**
    1   (0 0 0 1)
    4   (0 1 0 0)
           ↑   ↑
    求两个数的汉明距离
    The above arrows point to positions where the corresponding bits are different.</pre>
    <pre class="pure-highlightjs"><code class="">class Solution {
    public:
        int hammingDistance(int x, int y) {
            int dif = x^y;    //取相反位
            int count = 0;    //计算1个个数
            while(dif!=0){
                dif&amp;=(dif-1);
                count++;
            }
            return count;
        }
    };`

&nbsp;
