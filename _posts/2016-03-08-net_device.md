---
layout: post
title: "net_device结构学习"
date: 2016-03-08 15:35
author: imwack
comments: true
categories: [Linux]
tags: [linux]
---
net_device数据结构是用于存储网络设备的所有信息。具体参考文件linux/netdevice.h

net_device结构的字段主要分为以下几个方面：


1 配置


	char name[IFNAMSIZ]; //设备名

	unsigned long mem_end; /* shared mem end */

	unsigned long mem_start; /* shared mem start */

	unsigned long base_addr; /* device I/O address */

	unsigned int irq; /* device IRQ number */

	unsigned mtu; /* interface MTU value */ `




2 设备状态

	unsigned long state; //状态
	unsigned long last_rx; /* Time of last Rx */




3 列表管理

	/* device name hash chain */
	struct hlist_node name_hlist;




4流量管理

5功能专用

6通用

	/* Number of references to this device 引用计数器*/
	atomic_t refcnt ____cacheline_aligned_in_smp;
	int watchdog_timeo; /* used by dev_watchdog() */
	struct timer_list watchdog_timer;




*   函数指针
由于水平有限，刚开始学习，很多东西还无法理解，以后再做补充，先记录这么多~

注: 参考&lt;深入理解linux网络技术内幕&gt;一书第二章net_device结构。
