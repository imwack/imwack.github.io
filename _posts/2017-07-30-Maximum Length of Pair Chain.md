---
layout: post
title: "646. Maximum Length of Pair Chain"
date: 2017-07-31 20:50
author: wack
comments: true
categories: [Algorithm,LeetCode]
tags: [Algorithm,LeetCode]
---

You are given n pairs of numbers. In every pair, the first number is always smaller than the second number.

Now, we define a pair (c, d) can follow another pair (a, b) if and only if b < c. Chain of pairs can be formed in this fashion.

Given a set of pairs, find the length longest chain which can be formed. You needn't use up all the given pairs. You can select pairs in any order.

	Example 1:
	Input: [[1,2], [2,3], [3,4]]
	Output: 2
Explanation: The longest chain is [1,2] -> [3,4]

Note:The number of given pairs will be in the range [1, 1000].


{% highlight c++ %}
class Solution {
public:
    int findLongestChain(vector<vector<int>>& pairs) {
        sort(pairs.begin(),pairs.end(),[](vector<int> v1,vector<int> v2){
            return v1[1]<v2[1];
        }
        );
        int tail = pairs[0][1];
        int count = 1;
        for(int i=1;i<pairs.size();++i){
            if(pairs[i][0] > tail){
                count++;
                tail = pairs[i][1];
            }
        }
        return count;
    }
};
{% endhighlight %}