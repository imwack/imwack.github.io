---
layout: post
title: "23.从上往下打印二叉树"
date: 2016-10-15 10:54
author: imwack
comments: true
categories: [剑指offer]
tags: []
---
<h2 class="subject-item-title">题目描述


<div class="subject-question">从上往下打印出二叉树的每个节点，同层节点从左至右打印。</div>
<div class="subject-question">


    vector<int> PrintFromTopToBottom(TreeNode *root) {
        queue<TreeNode *> q;
            vector<int> result;
            if(!root)
                return result;
            else
                q.push(root);
            while(!q.empty()){
                TreeNode *node = q.front();
                result.push_back(node->val);
                if(node->left)
                    q.push(node->left);
                if(node->right)
                    q.push(node->right);
                q.pop();
            }
            return result;
        }
