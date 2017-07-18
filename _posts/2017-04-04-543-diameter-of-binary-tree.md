---
layout: post
title: "543. Diameter of Binary Tree"
date: 2017-04-04 10:23
author: imwack
comments: true
categories: [LeetCode]
tags: []
---
给定二叉树，求其最长半径，即两个节点间最长路径长度(可以不经过root）

**Example:**
Given a binary tree


          1
             / \
            2   3
           / \     
          4   5    
 Return **3**, which is the length of the path [4,2,1,3] or [5,2,1,3].
    
**Solution：**
    
这题实际上是求节点最大的左右孩子高度之和，代码如下

     /**
     * Definition for a binary tree node.
     * struct TreeNode {
     *     int val;
     *     TreeNode *left;
     *     TreeNode *right;
     *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
     * };
     */
    class Solution {
    public:
        int Diameter=0;
        int dfs(TreeNode *root){
            if(root==NULL) return 0;
            int l = dfs(root-&gt;left);   //左孩子高度
            int r = dfs(root-&gt;right);  //右孩子高度
            if(l+r&gt;Diameter) Diameter=l+r;  //半径
            return max(l,r)+1;
        }
        int diameterOfBinaryTree(TreeNode* root) {
            dfs(root);
            return Diameter;
        }
    };`

&nbsp;

&nbsp;

&nbsp;
