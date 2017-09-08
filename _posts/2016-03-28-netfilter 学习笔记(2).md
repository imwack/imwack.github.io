---
layout: post
title: "netfilter 学习笔记(2) - proc(上)"
date: 2016-03-28 14:28
author: imwack
comments: true
categories: [Linux, 内核]
tags: []
---
之前有一篇博文提到了使用proc的方法来使内核和用户空间交互，在linux 3.0版本以后的proc的一些函数似乎有了一些修改，原来的一些函数如create_proc_read_entry、create_proc_entry好像在我使用对linux版本（3.14内核版本）中已经不复存在了，具体参考linux/proc_fs.h文件。 现在一般使用proc_create这个函数，就此我们来学习一下其使用方法。

1.创建proc文件夹。
struct proc_dir_entry *proc_mkdir(const char *name, struct proc_dir_entry *parent);
name即创建的文件夹名称。
parent是创建节点的父节点。如果是在/proc目录下创建文件夹，parent为NULL。

例如struct proc_dir_entry *test_dir = proc_mkdir("test_dir", NULL);

2.proc文件的创建。
static inline struct proc_dir_entry *proc_create(const char *name, mode_t mode, struct proc_dir_entry *parent, const struct file_operations *proc_fops);
name就是要创建的文件名。
mode是文件的访问权限。
parent是父文件夹的       proc_dir_entry对象就是上面创建对对象。
proc_fops就是该文件的操作函数了。

例如：struct proc_dir_entry *test_file = proc_create("test_file", 0x0644, test_dir, test_proc_fops);

3.下面所proc_fops的定义。

例如
static const struct file_operations test_proc_fops = {
.open  = test_proc_open,
.read  = seq_read,
.write  = test_proc_write,
.llseek  = seq_lseek,
.release = single_release,
};

4.open文件打开

static int test_proc_open(struct inode *inode, struct file *file)
{
return single_open(file, test_proc_show, inode-&gt;i_private);
}

5.show

static int test_proc_show(struct seq_file *seq, void *v)
{
return seq_printf(seq, "Hello proc!\n");
}

6.write

static ssize_t test_proc_write(struct file *file, const char __user *buffer,
size_t count, loff_t *pos)
{
return count;
}

7.删除

remove_proc_entry(TEST_PROC_FILE, NULL);

对于show 和 write的一些参数 还不太了解以后在做补充吧。整个流程大约所上面几个步骤。

&nbsp;
