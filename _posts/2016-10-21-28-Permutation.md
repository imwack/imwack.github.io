---
layout: post
title: "28.字符串的排列"
date: 2016-10-21 12:08
author: imwack
comments: true
categories: [剑指offer]
tags: []
---
<h2 class="subject-item-title">题目描述

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。 结果请按字母顺序输出。

	class Solution {
    public:
        vector<string> ret;
        vector<string> Permutation(string str) {
            if(str.length()==0)
                return ret;
            Permutation(str,0);    
            sort(ret.begin(), ret.end());
            return ret;
        }
        void Permutation(string str, int begin){
            if(begin<str.length()){
                //第一个字符和后面所有字符交换
                for(int i=begin;i<str.length();i++){
                       if(i!=begin || str[i]==str[begin])//有重复字符时，跳过
                        continue;            
                    swap(str[i],str[begin]);               
                    Permutation(str,begin+1);   //递归
                    swap(str[i],str[begin]);                   
                }               
            }
            else
                ret.push_back(str);
        }  
        
    };
