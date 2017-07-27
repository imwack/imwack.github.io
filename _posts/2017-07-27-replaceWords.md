---
layout: post
title: "648.replaceWords"
date: 2017-07-27 20:48
author: wack
comments: true
categories: [Algorithm,LeetCode]
tags: [Algorithm,LeetCode]
---

In English, we have a concept called root, which can be followed by some other words to form another longer word - let's call this word successor. For example, the root an, followed by other, which can form another word another.

Now, given a dictionary consisting of many roots and a sentence. You need to replace all the successor in the sentence with the root forming it. If a successor has many roots can form it, replace it with the root with the shortest length.

You need to output the sentence after the replacement.

Example 1:
Input: dict = ["cat", "bat", "rat"]
sentence = "the cattle was rattled by the battery"
Output: "the cat was rat by the bat"
Note:
The input will only have lower-case letters.
1 <= dict words number <= 1000
1 <= sentence words number <= 1000
1 <= root length <= 100
1 <= sentence words length <= 1000

	class Trie {
		bool isEnd = false; // 是否是结尾
		Trie *child[26] = {}; //子节点
	public:
		void Insert(string word, int pos) {
			if (pos == word.size()) {
				isEnd = true;
				return;
			}
			int index = word[pos] - 'a';
			if (child[index] == nullptr) {
				child[index] = new Trie();
			}
			child[index]->Insert(word, pos + 1);
		}
		//若是开头 则返回开头的串，否则返回原串
		string isStartWith(string word, int pos) {
			if (pos == word.size()) {
				return word;
			}
			int index = word[pos] - 'a';
			if (child[index] == nullptr) return word;
			else if (child[index]->isEnd) 
				return word.substr(0, pos+1);
			else
				return child[index]->isStartWith(word, pos + 1);
		}
	};
	class Solution {
	public:
	string replaceWords(vector<string>& dict, string sentence) {
		Trie root;
		string ret;
		for (string word : dict) {
			root.Insert(word,0); //先构造字典树
		}
		stringstream ss;
		ss << sentence;
		string word;
		while (ss>>word)
		{
			if(ret.size())
				ret+=" "+ root.isStartWith(word,0);
			else
				ret+= root.isStartWith(word, 0);
		}
		return ret;
	}
	};
