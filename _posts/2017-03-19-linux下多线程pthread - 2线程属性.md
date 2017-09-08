---
layout: post
title: "linux下多线程pthread - 2线程属性"
date: 2017-03-19 12:23
author: imwack
comments: true
categories: [Linux]
tags: []
---
这里先介绍一下pthread_attr_t描述线程属性，这个结构体在源码中并不能看到。。。


   typedef struct
    {
             int                           detachstate;     线程的分离状态
             int                          schedpolicy;   线程调度策略
             struct sched_param      schedparam;   线程的调度参数
             int                          inheritsched;    线程的继承性
             int                          scope;          线程的作用域
             size_t                      guardsize; 线程栈末尾的警戒缓冲区大小
             int                          stackaddr_set;
             void *                     stackaddr;      线程栈的位置
             size_t                      stacksize;       线程栈的大小
    }pthread_attr_t;`</pre>
    测试代码
    <pre class="pure-highlightjs"><code class="">#include&lt;iostream&gt;
    #include&lt;unistd.h&gt;
    #include&lt;pthread.h&gt;
    using namespace std;
    //增加了 创建线程属性
    void *thread(void *args)
    {
        for(int i=0;i&lt;3;i++){
            sleep(1);
            cout&lt;&lt;"This is a pthread."&lt;&lt;endl;
        }
    }
    void *thread_with_arg(void *args){
        int i = *((int *)args);
        cout&lt;&lt;"thread_with_arg:"&lt;&lt;i&lt;&lt;endl;
    }
    int main(){
        pthread_t id[3];
        int args = 666;
        **pthread_attr_t** attr;    //线程属性结构体
        **pthread_attr_init**(&amp;attr);   //初始化线程属性
        **pthread_attr_setdetachstate**(&amp;attr,**PTHREAD_CREATE_JOINABLE**);  //设置你想指定线程属性参数，这个参数表示这个参数可以join连接，主程序可以等线程结束再去做某事
        for(int i=0;i&lt;3;i++){
            int ret = pthread_create(&amp;id[i],&amp;**attr**,thread_with_arg,(void*)&amp;(**id**[i]));
            if(ret){    //ret!=0 means create thread error
                cout&lt;&lt;"pthread_create error:"&lt;&lt;ret&lt;&lt;endl;
                return 1;
            }
        }
        pthread_attr_destroy(&amp;attr); //释放内存
        void *status;
        for(int i=0;i&lt;3;i++){
            int ret = pthread_join(id[i],&amp;**status**);  //wait for thread :id finish获取每个线程退出信息
            if(ret!=0)
                cout&lt;&lt;"pthread_join,error code:"&lt;&lt;ret&lt;&lt;endl;
            else
                cout&lt;&lt;"pthread_join,status:"&lt;&lt;(long)status&lt;&lt;endl;
        }
    
        pthread_exit(NULL);
        return 0;
    }
    `</pre>
    结果
    <pre class="pure-highlightjs"><code class="">thread_with_arg:-97827072
    pthread_join,status:6299840
    thread_with_arg:-106219776
    pthread_join,status:6299840
    thread_with_arg:-114612480
    pthread_join,status:6299840
    `

&nbsp;
