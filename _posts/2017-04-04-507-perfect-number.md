---
layout: post
title: "507. Perfect Number"
date: 2017-04-04 10:27
author: imwack
comments: true
categories: [LeetCode]
tags: []
---
We define the Perfect Number is a **positive** integer that is equal to the sum of all its **positive** divisors except itself.   

**给定正整数如果它等于其除数和则为Perfect**

Now, given an **integer** n, write a function that returns true when it is a perfect number and false when it is not.

**Example:**


**Input:** 28

**Output:** True

**Explanation:** 28 = 1 + 2 + 4 + 7 + 14

**Solution:**

    	bool checkPerfectNumber(int num) {
            if(num==1) return false;
            int sum=1;
            for(int i=2;i*i&lt;num;i++){
                if(num%i==0){
                    sum+=i;
                    if (i*i!= num) {      //考虑平方数
                        sum += num / i;
                    }
                }
            }
            return sum==num;
        }`</pre>
    </div>
 根据 [https://en.wikipedia.org/wiki/List_of_perfect_numbers](https://en.wikipedia.org/wiki/List_of_perfect_numbers) 实际上在int型范围内只有5个Perfect Number，可以直接（这很皮）
    <pre class="markdown-highlight"><code class="hljs ruby"><span class="hljs-keyword">return</span> ((num == <span class="hljs-number">6</span>) <span class="hljs-params">||</span> (num == <span class="hljs-number">28</span>) <span class="hljs-params">||</span> (num == <span class="hljs-number">496</span>) <span class="hljs-params">||</span> (num == <span class="hljs-number">8128</span>) <span class="hljs-params">||</span> (num == <span class="hljs-number">33550336</span>));`

