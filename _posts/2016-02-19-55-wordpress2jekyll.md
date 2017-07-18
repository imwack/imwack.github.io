---
layout: post
title: "WordPress转移到jekyll"
date: 2017-7-16 18:28
author: wack
comments: true
categories: [Jekyll]
tags: [Jekyll]
---
 
今天把WordPress转移到了GitHub Page上，费了很大功夫，故记录一下~

1.去官网下载ruby，我用的2.4的

2.安装jekyll

		gem install jekyll
3.建一个项目，或者直接去网上找一个模板

	$ jekyll new myblog
	$ cd myblog
	$ jekyll serve 启动服务 默认http://localhost:4000
这里有不少模板 [http://jekyllthemes.org/](http://jekyllthemes.org/)可以找一个免费的下下来

4.去GitHub上建一个仓库名称为： **用户名.github.io**   
  &nbsp;&nbsp;&nbsp;这里一定不能写错。。

5.把代码同步上去

---
jekyll一般的目录结构如下


	├── _config.yml  	#配置文件
	├── _drafts			#草稿
	|   ├── begin-with-the-crazy-ideas.textile
	|   └── on-simplicity-in-technology.markdown
	├── _includes		#加载这些包含部分到你的布局或者文章中以方便重用
	|   ├── footer.html
	|   └── header.html		
	├── _layouts		#包裹在文章外部的模板
	|   ├── default.html
	|   └── post.html
	├── _posts			#文章
	|   ├── 2007-10-29-why-every-programmer-should-play-nethack.textile
	|   └── 2009-04-26-barcamp-boston-4-roundup.textile
可以参考官网 [http://jekyllcn.com/docs/structure/](http://jekyllcn.com/docs/structure/)

想要绑定域名的需要在项目设置里设置域名，并且设置DNS解析，先这样，比较晚了明天还要工作。

