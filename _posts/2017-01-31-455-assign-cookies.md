---
layout: post
title: "455. Assign Cookies"
date: 2017-01-31 16:41
author: imwack
comments: true
categories: [LeetCode]
tags: []
---
Assume you are an awesome parent and want to give your children some cookies. But, you should give each child at most one cookie. Each child i has a greed factor g<sub>i</sub>, which is the minimum size of a cookie that the child will be content with; and each cookie j has a size s<sub>j</sub>. If s<sub>j</sub> &gt;= g<sub>i</sub>, we can assign the cookie j to the child i, and the child i will be content. Your goal is to maximize the number of your content children and output the maximum number.

**Example 1:**


**Input:** [1,2,3], [1,1]
    
    **Output:** 1
    
    **Explanation:** You have 3 children and 2 cookies. The greed factors of 3 children are 1, 2, 3. 
    And even though you have 2 cookies, since their size is both 1, you could only make the child whose greed factor is 1 content.
    You need to output 1.</pre>
    给定数组A表示孩子需求的饼干数目，数组B表示每份饼干的数目，求出最多能满足孩子需求的数量。
    
    My Solution ： 先排序，从小到大依次分配，先分配需求少的。
    <pre class="pure-highlightjs"><code class="">    int findContentChildren(vector&lt;int&gt;&amp; g, vector&lt;int&gt;&amp; s) {
            int count = 0;
            int index1 = 0;
            int index2 = 0;
            sort(g.begin(),g.end());
            sort(s.begin(),s.end());
            while(index1&lt;g.size() &amp;&amp; index2&lt;s.size()){
                if(s[index2]&gt;=g[index1]){
                    count++;
                    index1++;
                    index2++;
                }else{
                    index2++;
                }
            }
            return count;
        }`

&nbsp;
