---
layout: post
title: "652. Find Duplicate Subtrees"
date: 2017-07-30 12:48
author: wack
comments: true
categories: [Algorithm,LeetCode]
tags: [Algorithm,LeetCode]
---
	Example 1: 
	        1
	       / \
	      2   3
	     /   / \
	    4   2   4
	       /
	      4
	The following are two duplicate subtrees:
	      2
	     /
	    4
	and
	    4

{% highlight c++ %}

class Solution {
public:

    string PreOrder(TreeNode *root, map<string, vector<TreeNode*>> &m){
        if (root == nullptr) return "#";
        string temp = to_string(root->val) + PreOrder(root->left,m) + PreOrder(root->right,m);
        m[temp].push_back(root);
        return temp;
    }
    vector<TreeNode*> findDuplicateSubtrees(TreeNode* root) {
        vector<TreeNode*> ret;
        map<string, vector<TreeNode*>> m;
        PreOrder(root, m);
        for (auto it = m.begin(); it != m.end(); it++){
            if (it->second.size() > 1)
                ret.push_back(it->second[0]);
        }

        return ret;
    }
};

{% endhighlight %}