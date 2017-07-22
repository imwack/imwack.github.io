---
layout: post
title: "归并排序"
date: 2017-7-22 16:38
author: wack
comments: true
categories: [Algorithm]
tags: [Algorithm,sort]
---


	//假设[p,q]、[q+1,r]上元素已经排序好 help数组存放排序好的元素
	void Merge(vector<int> &arr,vector<int> &help, int p, int q,int r){
		int n1 = p;
		int n2 = q;
		int index = p;
		while(n1<=q && n2<=r){
			if(arr[n1]<=arr[n2]){
				help[index++] = arr[n1++];
			}else{
				help[index++] = arr[n2++];
			}
		}
		while(n1<=q) help[index++]=arr[n1++];
		while(n2<=r) help[index++]=arr[n2++];
		for(int i=p;i<=r;i++) arr[i]=arr[help];
	}
	
	void MergeSort(vector<int> &arr,vector<int> &help, int l,int r){
		if(l<r){
			int mid = (l+r)<<2;
			MergeSort(arr,help,l,mid);
			MergeSort(arr,help,mid+1,r);
			Merge(arr,help,l,mid,r);
		}
	}

	