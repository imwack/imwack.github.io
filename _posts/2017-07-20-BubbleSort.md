---
layout: post
title: "冒泡排序"
date: 2017-7-20 14:55
author: wack
comments: true
categories: [Algorithm]
tags: [Algorithm,sort]
---

冒泡排序每次将相邻两个数比较，假设递增排序，大的则往后扔，这样一轮下来就可以找到一个最大的，如此反复,复杂的O(n^2).

	void bubbleSort(vector<int> v){
		int n = v.size();
		for(int i=0;i<n-1;i++){	//循环n-1次
			for(int j=0;j<n-i-1;j++){	//循环长度为n-i-1次
				if(v[j]<v[j+1])	//比较相邻的
					swap(v[j],v[j+1]);
			}
		}
	}
改进：每次交换可以记录下交换的位置，如果已经没有交换了则没有必要继续循环

	void bubbleSort(vector<int> v){
		int n = v.size();
		int i = n-1;
		while(i>0){	//循环n-1次+
			int pos = 0;
			for(int j=0;j<i;j++){	//循环长度为i次 
				if(v[j]<v[j+1]){	//比较相邻的
					pos = j;		//记录最后一次交换位置
					swap(v[j],v[j+1]);
				}	
			}
			i = pos;
		}
	}