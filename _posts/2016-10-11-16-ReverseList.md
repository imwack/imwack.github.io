---
layout: post
title: "16.反转链表"
date: 2016-10-11 16:55
author: imwack
comments: true
categories: [剑指offer]
tags: []
---

输入一个链表，反转链表后，输出链表的所有元素。


	 ListNode* ReverseList(ListNode* pHead) {
        if(!pHead || !pHead-&gt;next)
                return pHead;
            ListNode *pre=NULL,*pCur=pHead;
            while(pCur){
                ListNode *pNext = pCur-&gt;next;
                pCur-&gt;next = pre;
                pre = pCur;
                pCur = pNext;
                
            }
            return pre;
        }`

&nbsp;

&nbsp;
