---
layout: post
title: "proc_create_data 使用"
date: 2016-04-18 15:03
author: imwack
comments: true
categories: [Linux, 内核]
tags: []
---
由于在linux内核3.0版本以上，一些老版本到proc方法已经被删除，研究了三天才修改了以前到一些代码，所以特此记录。


<code class="">#include &lt;linux/module.h&gt;
    #include &lt;linux/sched.h&gt;
    #include &lt;linux/proc_fs.h&gt;
    #include &lt;linux/seq_file.h&gt;
    #include &lt;linux/uaccess.h&gt;
    #include &lt;linux/slab.h&gt;
    
    static char *str = NULL;
    struct proc_dir_entry * KksProcDir = NULL;
    // linux/seq_file.h
    // void * (*start) (struct seq_file *m, loff_t *pos);
    // void (*stop) (struct seq_file *m, void *v);
    // void * (*next) (struct seq_file *m, void *v, loff_t *pos);
    // int (*show) (struct seq_file *m, void *v);
    
    /**
    * fuction: seq_operations -&gt; start
    */
    static void *my_seq_start(struct seq_file *m, loff_t *pos)
    {
        if (0 == *pos)
        {
            ++*pos;
            return (void *)1; // return anything but NULL, just for test
        }
        return NULL;
    }
    
    /**
    * fuction: seq_operations -&gt; next
    */
    static void *my_seq_next(struct seq_file *m, void *v, loff_t *pos)
    {
        // only once, so no next
        return NULL;
    }
    
    /**
    * fuction: seq_operations -&gt; stop
    */
    static void my_seq_stop(struct seq_file *m, void *v)
    {
        // clean sth.
        // nothing to do
    }
    
    /**
    * fuction: seq_operations -&gt; show
    */
    static int my_seq_show(struct seq_file *m, void *v)
    {
        seq_printf(m, "current kernel time is %llu\n", (unsigned long long) get_jiffies_64());
        seq_printf(m, "str is %s\n", str);
    
        return 0; //!! must be 0, or will show nothing T.T
    }
    
    // global var
    static struct seq_operations my_seq_fops = 
    {
        .start    = my_seq_start,
        .next    = my_seq_next,
        .stop    = my_seq_stop,
        .show    = my_seq_show,
    };
    
    /**
    * fuction: file_operations -&gt; open
    */
    static int proc_seq_open(struct inode *inode, struct file *file)
    {
        return seq_open(file, &amp;my_seq_fops);
    }
    
    /**
    * fuction: file_operations -&gt; write
    */
    static ssize_t proc_seq_write(struct file *file, const char __user *buffer, size_t count, loff_t *f_pos)
    {
        //分配临时缓冲区
        char *tmp = kzalloc((count+1), GFP_KERNEL);
        if (!tmp)
            return -ENOMEM;
    
        //将用户态write的字符串拷贝到内核空间
        //copy_to|from_user(to,from,cnt)
        if (copy_from_user(tmp, buffer, count)) {
            kfree(tmp);
            return -EFAULT;
        }
    
        //将str的旧空间释放，然后将tmp赋值给str
        kfree(str);
        str = tmp;
    
        return count;
    }
    
    static struct file_operations proc_seq_fops = 
    {
        .owner    = THIS_MODULE,
        .open        = proc_seq_open,
        .read        = seq_read,
        .write        = proc_seq_write,
        .llseek        = seq_lseek,
        .release    = seq_release,
    };
    
    static int __init my_init(void)
    {
        struct proc_dir_entry *file;
        KksProcDir = proc_mkdir("kks",NULL);
        // create "/proc/proc_seq" file
        file = proc_create_data(
            "jif",        // name
            0666,        // mode
            KksProcDir,        // parent dir_entry
            &amp;proc_seq_fops,    // file_operations
            NULL        // data
            );
        if (NULL == file)
        {
            printk("Count not create /proc/jif file!\n");
            return -ENOMEM;
        }
    
        return 0;
    }
    
    static void __exit my_exit(void)
    {
        remove_proc_entry("jif", NULL);
        kfree(str);
    }
    
    module_init(my_init);
    module_exit(my_exit);
    
    MODULE_AUTHOR("wack");
    MODULE_LICENSE("GPL");
    `

&nbsp;
