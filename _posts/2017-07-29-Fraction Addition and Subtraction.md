---
layout: post
title: "592. Fraction Addition and Subtraction"
date: 2017-07-29 20:48
author: wack
comments: true
categories: [Algorithm,LeetCode]
tags: [Algorithm,LeetCode]
---

Given a string representing an expression of fraction addition and subtraction, you need to return the calculation result in string format. The final result should be irreducible fraction. If your final result is an integer, say 2, you need to change it to the format of fraction that has denominator 1. So in this case, 2 should be converted to 2/1.

	Example 1:
	Input:"-1/2+1/2"
	Output: "0/1"
大概意思就是计算分数的加减法


<code class="hljs livecodeserver">{% highlight ruby %}

	class Solution {
	public:
	    int gcd(int a, int b){
		if (a < b) swap(a, b);
	    if(b==0) return a;
		int c = a%b;
		while (c!=0)
		{
			a = b;
			b = c;
			c = a%b;
		}
		return b;
	
	}
	string fractionAddition(string expression) {
		stringstream ss(expression);
	
		int a, b,A=0,B=1,g;
		char _;
		while (ss>>a>>_>>b)
		{
			A = A*b + a*B;
			B = b*B;
			g = abs(gcd(A, B));
	        A /= g;
			B /= g;
	
		}
		return  to_string(A) + '/' + to_string(B);
	}
	};

{% endhighlight %}</code>