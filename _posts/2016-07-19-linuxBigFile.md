---
layout: post
title: "Linux下大文件去重和排序"
date: 2016-07-19 11:23
author: imwack
comments: true
categories: [Linux]
tags: []
---
最近做项目遇到了一个问题，在一个很大的文本文件中，按行排序和去除重复的行，文件很大有十几G，在Windows下都无法打开。查阅了一些资料，得知linux下有很高效的处理方式，比写python脚本要快很多。

另外由于文件很大，使用分治法的思想，先将文件分割，单独排序去重，再合并，这样就不用担心内存不够的问题。

首先分割文件（按大小分割，用行来处理很慢）


	split -b 200m  xx.txt Prefix_ #200M每个文件`</pre>
    排序
    for file in Prefix_*
    do
    {
         sort $file> sort_$file
    };
    done
    合并去重
    sort -smu sort_* > result
    删除不用的文件
    rm Prefix_*
    rm sort_*`

&nbsp;
