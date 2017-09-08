---
layout: post
title: "linux下多线程pthread - 1基础"
date: 2017-03-19 12:18
author: imwack
comments: true
categories: [Linux]
tags: []
---
&nbsp;


    #include&lt;iostream&gt;
    #include&lt;unistd.h&gt;
    #include&lt;pthread.h&gt;
    using namespace std;
    
    void *thread(void *args)     //线程函数
    {
        for(int i=0;i&lt;3;i++){
            sleep(1);
            cout&lt;&lt;"This is a pthread."&lt;&lt;endl;
        }
    }
    void *thread_with_arg(void *args){  //线程函数（处理传入参数）
        int i = *((int *)args);                        //参数类型转换
        cout&lt;&lt;"thread_with_arg:"&lt;&lt;i&lt;&lt;endl;
    }
    int main(){
        pthread_t id;  //声明一个线程
        int args = 666; //传入参数测试
        int ret = pthread_create(&amp;id,NULL,thread_with_arg,(void*)&amp;args); //创建线程
        /*
        extern int pthread_create (pthread_t *__restrict __newthread,   //线程id
                   const pthread_attr_t *__restrict __attr,      //线程属性，下文描述
                   void *(*__start_routine) (void *),               //线程执行函数
                   void *__restrict __arg) __THROWNL __nonnull ((1, 3));   //传入参数
        */
        if(ret){    //ret！=0 表示创建失败
            cout&lt;&lt;"pthread_create error:"&lt;&lt;ret&lt;&lt;endl;
            return 1;
        }
        for(int i=0;i&lt;3;i++){
            cout&lt;&lt;"this is main process"&lt;&lt;endl;
            sleep(1);
        }
        pthread_join(id,NULL);  //等待线程id结束
        pthread_exit(NULL);
        return 0;
    }
    `</pre>
    特别注明，g++编译需要加参数-lpthread因为要处理多线程操作相关的静态链接库文件
    <pre class="pure-highlightjs"><code class="">结果，因为是多线程所以输出会混乱
    this is main process
    thread_with_arg:666
    this is main process
    this is main process
    `

&nbsp;
