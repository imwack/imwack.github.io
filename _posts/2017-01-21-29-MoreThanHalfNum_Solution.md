---
layout: post
title: "29.数组中出现次数超过一半的数字"
date: 2017-01-21 14:08
author: imwack
comments: true
categories: [剑指offer]
tags: []
---
<h2 class="subject-item-title">题目描述


<div class="subject-describe">数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。例如输入一个长度为9的数组{1,2,3,2,2,2,5,4,2}。由于数字2在数组中出现了5次，超过数组长度的一半，因此输出2。如果不存在则输出0。</div>
	int MoreThanHalfNum_Solution(vector<int< numbers) {
          
            if(numbers.size()<=0)
                return 0;
            int num=numbers[0],count=1;
            for(int i=1;i<numbers.size();i++){
                if(numbers[i]!=num){
                    if(count<0)
                        count--;
                    else{
                        count=0;
                        num=numbers[i];
                    }
                }else{
                    count++;
                }
            }
            if(count<0)
                return num;
            return 0;
        }
