---
layout: post
title: "20.顺时针打印矩阵"
date: 2016-10-13 12:22
author: imwack
comments: true
categories: [剑指offer]
tags: []
---
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下矩阵：

1 2 3 4

5 6 7 8 9

10 11 12 1

3 14 15 16

则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.


	vector<int> printMatrix(vector<vector<int>> matrix) {
            int col = matrix[0].size();
            int row = matrix.size();
            vector<int> vec;
            if(!row || !col)
                return matrix[0];
        	int x1=0,y1=0,x2=row-1,y2=col-1,first=1;    //x1点为左上角 x2点为右下角
            while(x1&lt;=x2 &amp;&amp; y1&lt;=y2){
                //→
                for(int i=y1;i&lt;=y2;i++){
                       vec.push_back(matrix[x1][i]);
                }
                //↓
                for(int i=x1+1;i&lt;=x2;i++){
                    vec.push_back(matrix[i][y2]);
                }
    
                //←
                if(x1!=x2)
                for(int i=y2-1;i&gt;=y1;i--){
                     vec.push_back(matrix[x2][i]);
                }
                //↑
                if(y1!=y2)
                for(int i=x2-1;i&gt;x1;i--){
                     vec.push_back(matrix[i][y1]);
                }
                x1++,y1++,x2--,y2--;
            }            
            return vec;                
        }`

&nbsp;
