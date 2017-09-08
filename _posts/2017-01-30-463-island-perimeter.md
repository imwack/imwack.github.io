---
layout: post
title: "463. Island Perimeter"
date: 2017-01-30 23:43
author: imwack
comments: true
categories: [LeetCode]
tags: []
---


	[[0,1,0,0],
     [1,1,1,0],
     [0,1,0,0],
     [1,1,0,0]]
    
    Answer: 16
    Explanation: The perimeter is the 16 yellow stripes in the image below:
    ![](https://leetcode.com/static/images/problemset/island.png)</pre>
    题目意思就是计算岛屿（矩阵中为1 ）的面积
    <pre class="pure-highlightjs"><code class="">class Solution {
    public:
        int islandPerimeter(vector&lt;vector&lt;int&gt;&gt;&amp; grid) {
            int row = grid.size();
            int col = grid[0].size();
            int count = 0;
            for(int i=0;i&lt;row;i++){
                for(int j=0;j&lt;col;j++){
                    if(grid[i][j]==1){
                        count+=perimeter(grid,i,j,row,col);
                    }
                }
            }
            return count;
        }
        int perimeter(vector&lt;vector&lt;int&gt;&gt;&amp; grid,int i,int j,int row,int col){
            int count = 0;
            //判断上下左右是不是也是1
            if(i-1&lt;0 || grid[i-1][j]==0) count++;    
            if(i+1&gt;=row || grid[i+1][j]==0) count++;
            if(j-1&lt;0 || grid[i][j-1]==0) count++;
            if(j+1&gt;=col || grid[i][j+1]==0) count++;
            return count;
        }
    };`

&nbsp;
