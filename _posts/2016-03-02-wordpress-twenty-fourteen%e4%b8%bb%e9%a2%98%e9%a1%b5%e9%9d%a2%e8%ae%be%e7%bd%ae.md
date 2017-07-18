---
layout: post
title: "WordPress Twenty-Fourteen主题页面设置"
date: 2016-03-02 22:27
author: imwack
comments: true
categories: []
tags: [WordPress]
---


######   之前一直觉得这个主题的颜色我比较喜欢，可是有个小小的缺点就是右边有一块留白，我反而感觉不好看，网上找了一些方法设置把它去除，下面是方法。




###### 修改主题的样式表文件style.css,路径在WordPress目录下的
<span style="color: #ff0000;">wp-content/themes/twentyfourteen/</span>


1.设置页面宽占整个页面， 在文件最后加上


<code class="">.site,
    .site-header {
        max-width: 100%;
    }`</pre>

2.要想设置中间的文章部分的宽度需要加上以下代码
    <pre class="pure-highlightjs"><code class="">.site-content .entry-header,
    .site-content .entry-content,
    .site-content .entry-summary,
    .site-content .entry-meta, .page-content {
    /* Original max-width: 474px */
        max-width: 80%;
    }`</pre>

 3.设置页面上方图像宽度

    <pre class="pure-highlightjs"><code class="">#site-header img{
        width:100%;
    } 


但是如果页面右侧有工具栏的话，这样的设置还是会导致中间页面有一部分空白，一行的长度还是很短，看了下页面的样式表总是有一部分margin在两侧，但是经过多次尝试都没有能改好，所以只能把右边的工具栏去掉了。。。
