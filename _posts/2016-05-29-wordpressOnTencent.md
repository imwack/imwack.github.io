---
layout: post
title: "腾讯云转移WordPress记录"
date: 2016-05-29 23:48
author: imwack
comments: true
categories: [WordPress]
tags: []
---
之前用的bwg服务器实在是比较卡，最近同学推荐腾讯云学生主机，非常便宜也很好用，配置过程比较艰辛，特此记录。

由于Ubuntu安装Apache总是有问题，后来换成了centos，配置也比较简单。

1.首先在腾讯云上进行实名认证，然后再进行学生认证 （大约三天），可以拿到优惠券，一个月只需1元，非常良心，顺便我也买了一个域名。

2.安装操作系统centos

3.Apache  

	#yum install httpd    #systemctl start httpd.service #启动

4.mysql 

因为centos yum源上已经没有mysql了这里需要使用第三方源才可以yum安装，也可以使用mariadb 和mysql一样 

	#yum install mariadb mariadb-server  
	#systemctl start mariadb.service #启动

5php 

	#yum install php-mysql php-gd libjpeg* php-ldap php-odbc php-pear php-xml php-xmlrpc php-mbstring php-bcmath php-mhash 这里一定要安装phpmysql扩展不然会有错误

6.Apache配置虚拟主机 
	#cd /etc/httpd/conf  
	#vim httpd.conf


	<VirtualHost> *:80
    DocumentRoot "/var/www/wack"   #根目录
    ServerName www.imwack.cn       #域名或者IP
    </VirtualHost>
    <Directory>
        Options FollowSymLinks
        AllowOverride None
        Order deny,allow
        allow from all
    Satisfy all


7.进入mysql 使用source 导入sql文件

8.重启Apache    #systemctl restart httpd.service

9.访问域名（需要在腾讯云上绑定IP）

拖了好几天才想起来把文章写上。。。今天不看电视早点休息~
