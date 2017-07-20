---
layout: post
title: "快速排序"
date: 2017-7-20 15:16
author: wack
comments: true
categories: [Algorithm]
tags: [Algorithm,sort]
---

	int quickSort(vector<int> arr, int l,int r){
		int flag = arr[l]; //立flag
		int i=l,j=r;
		while(i<j){
			while(i<j && arr[j]>flag) j--;	//从右往左找一个比flag小的
			if(i<j) arr[i++] = arr[j];
			while(i<j && arr[i]<flag) i++;	//找一个比flag大的
			if(i<j) arr[j--] = arr[i];
		}
		arr[i] = flag;
		quickSort(arr,l,i-1);
		quickSort(arr,i+1,r);
	}