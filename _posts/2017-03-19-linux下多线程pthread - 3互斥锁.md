---
layout: post
title: "linux下多线程pthread - 3互斥锁"
date: 2017-03-19 14:10
author: imwack
comments: true
categories: [Linux, 多线程]
tags: []
---
线程间同步是方式一般采用信号量、锁机制等，这里讨论互斥锁的使用。

1.结构体pthread_mutex_t表示互斥锁

2.初始化pthread_mutex_init(pthread_mutex_t *mutex, const pthread_mutexattr_t * attr) 可以指定锁的属性，和线程类似。

3.释放pthread_mutex_destory

4.上锁int pthread_mutex_lock(pthread_mutex_t *mutex)

5.解锁int pthread_mutex_unlock(pthread_mutex_t *mutex)

参考代码：


    #include&lt;iostream&gt;
    #include&lt;pthread.h&gt;
    #include&lt;sstream&gt;
    using namespace std;
    /*增加了对线程d异步处理，使用互斥锁*/
    int numOfThread = 5;
    long sum = 0;  //全局变量
    pthread_mutex_t mutex; //互斥锁
    ostringstream os;
    
    void* thread(void *args){
        cout&lt;&lt;"thread args:"&lt;&lt;*((int *)args)&lt;&lt;endl;
        pthread_mutex_lock(&amp;mutex); //l上锁
        cout&lt;&lt;"before add sum:"&lt;&lt;sum&lt;&lt;endl;
        sum+=*((int*)args); //互斥操作全局变量
        cout&lt;&lt;"after add sum:"&lt;&lt;sum&lt;&lt;endl;
        pthread_mutex_unlock(&amp;mutex);//解锁
        pthread_exit(0);
    }
    
    int main(){
        pthread_t id[5];
        int index[5]={0};
        pthread_attr_t attr;
        pthread_attr_init(&amp;attr);
        pthread_attr_setdetachstate(&amp;attr,PTHREAD_CREATE_JOINABLE);
        pthread_mutex_init(&amp;mutex,NULL);
        for(int i=0;i&lt;5;i++){
            index[i] = i;
            int ret = pthread_create(&amp;id[i],&amp;attr,thread,(void*)&amp;(index[i]));
            if(ret){    //ret!=0 means create thread error
                cout&lt;&lt;"pthread_create error:"&lt;&lt;ret&lt;&lt;endl;
                return 1;
            }
        }
        pthread_attr_destroy(&amp;attr);
            void *status;
        for(int i=0;i&lt;5;i++){
            int ret = pthread_join(id[i],&amp;status);  //wait for thread :id finish
            if(ret!=0)
                cout&lt;&lt;"pthread_join,error code:"&lt;&lt;ret&lt;&lt;endl;
            else
                cout&lt;&lt;"pthread_join,status:"&lt;&lt;(long)status&lt;&lt;endl;
        }
        cout&lt;&lt;"finally sum is"&lt;&lt;sum&lt;&lt;endl;
        pthread_mutex_destroy(&amp;mutex);//删除锁
    }
    `</pre>
    运行结果
    <pre class="pure-highlightjs"><code class="">thread args:0
    before add sum:0
    after add sum:0
    thread args:1
    before add sum:0
    after add sum:1
    thread args:2
    before add sum:1
    after add sum:3
    pthread_join,status:0
    thread args:4
    before add sum:3
    thread args:3after add sum:7
    pthread_join,status:0
    pthread_join,status:0
    before add sum:7
    after add sum:10
    pthread_join,status:0
    pthread_join,status:0
    finally sum is10
    `

可以看到互斥操作对全局变量计算结果是正确的
