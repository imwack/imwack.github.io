---
layout: post
title: "24.二叉搜索树的后序遍历序列"
date: 2016-10-15 18:28
author: imwack
comments: true
categories: [剑指offer]
tags: []
---
<h2 class="subject-item-title">题目描述


<div class="subject-question">输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。</div>



        bool VerifySquenceOfBST(vector<int> seq) {
            int len = seq.size();
            if(len==0)
                return false;
            return VerifySequence(seq,0,len-1);
        }
        bool VerifySequence(vector<int> seq,int left,int right){
            if(left == right)
                return true;
            int rootVal = seq[right];    //根节点值
            int mid;            
            for(mid=left;mid<right;mid++){        //找到左子树
                if(seq[mid]>rootVal)    //假设输入的数组的任意两个数字都互不相同。
                    break;
            }
            for(int j=mid;j<right;j++)    //右子树都大于根节点
                if(seq[j]<rootVal)
                    return false;
            if(mid==left || mid==right)
            {
                return VerifySequence(seq, left, right-1);
            }
            else
            return VerifySequence(seq,left,mid-1) &amp;&amp; VerifySequence(seq,mid,right);    
        }`

&nbsp;

</div>
