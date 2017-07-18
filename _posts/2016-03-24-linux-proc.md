---
layout: post
title: "linux用户空间与内核交互"
date: 2016-03-24 21:38
author: imwack
comments: true
categories: [Linux, 内核]
tags: []
---


##### procfs（proc文件系统）


这是一个虚拟文件系统，一般mount在/proc目录，以文件形式向用户空间输出内核信息。

proc可以使用/include/linux/proc_fs.h中的API进行注册和删除。常用的方法有create_proc_read_entry和remove_proc_entry，proc_net_fops_create用于创建文档，然后初始化文件操作处理函数


##### sysctl（/proc/sys目录）


此接口允许用户空间读取或修改内核变量的值。可以使用两种方式访问sysctl输出的变量：①使用sysctl系统调用 ②使用procfs

用户在/proc/sys目录下看到的文件实际是内核变量，该文件是以ctl_table结构定义，通过/kernel/sysctl.c中的register_sysctl_table注册。


##### sysfs（/sys文件系统）


2.6版本之后引入的新的文件系统。

&nbsp;
