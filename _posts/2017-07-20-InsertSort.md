---
layout: post
title: "插入排序"
date: 2017-7-20 14:38
author: wack
comments: true
categories: [Algorithm]
tags: [Algorithm,sort]
---

插入排序将数据插入到一个已经排序好的列表中，可以先将数组第一个数看做已经排序好，然后从第二个元素开始插入，

		e.g. 5,4,3,2,1

	i=2		(4,5),3,2,1
	i=3		(3,4,5),2,1
	i=4		(2,3,4,5),1
	i=5		(1,2,3,4,5)

因此复杂的是O(n^2)的，代码如下
	
	void InsertSort(vector<int> v)
	{
		int n = v.size();
		for(int i=1;i<n;i++){
			if(v[i]<v[i-1]){	//i元素比前一个小 需要往前扔
				int j=i-1;
				int x=a[i];		//记录当前元素i
				a[i] = a[i-1];	//i元素等于i-1
				while(j>=0 && x<a[j]){	//往前移动 直到找到一个比i小的
					a[j+1] = a[j];
					j--;		
				} 
				a[j+1] = x;
			}
		}
	}