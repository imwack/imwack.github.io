---
layout: post
title: "scikit-learn 安装"
date: 2017-03-23 22:56
author: imwack
comments: true
categories: [机器学习]
tags: []
---
**使用pip安装各个库**

1.安装Python pip 将...\Python27\Scripts;写入环境变量

2.安装numpy           pip install numpy

3.安装scipy               pip install scipy 有时候会出bug安装失败 应该是依赖库没满足 解决办法 **先安装下面的numpy+mkl库**

4.安装scikit-learn    pip install scikit-learn

**使用conda安装（推荐）**

1.下载对应版本conda [https://conda.io/miniconda.html ](https://conda.io/miniconda.html)不需要配环境变量

2.安装 <span class="fontstyle0">conda install scikit-learn</span>

3. 更新<span class="fontstyle0">conda update scikit-learn</span>

安装后发现scipy还是不能用，需要安装Numpy+MKL (建议先安装这个 别直接装numpy)

这里有很多Python模块安装包[http://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy](http://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy)

<span class="fontstyle0">**pandas** (http://pandas.pydata.org/pandas-docs/stable/index.html)</span><span class="fontstyle2">库，是一个开源
</span><span class="fontstyle0">Python</span><span class="fontstyle2">数据分析工具。</span><span class="fontstyle0">Windows</span><span class="fontstyle2">，</span><span class="fontstyle0">Linux</span><span class="fontstyle2">和</span><span class="fontstyle0">Mac OS</span><span class="fontstyle2">系统都可以用</span><span class="fontstyle0">pip</span><span class="fontstyle2">安装：
</span>
<p style="padding-left: 30px;"><span class="fontstyle3">pip install pandas</span>   <span class="fontstyle0">也可以用</span><span class="fontstyle1">conda</span><span class="fontstyle0">命令安装</span>

