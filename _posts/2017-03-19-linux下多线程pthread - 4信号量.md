---
layout: post
title: "linux下多线程pthread - 4信号量"
date: 2017-03-19 14:33
author: imwack
comments: true
categories: [Linux]
tags: []
---
线程间通信可以使用信号量的机制，linux下pthread_cond_t表示条件信号


## 1.初始化条件变量pthread_cond_init


int pthread_cond_init(pthread_cond_t *cv, const pthread_condattr_t *cattr); 成功则返回0


## 2.阻塞在条件变量上pthread_cond_wait


int pthread_cond_wait(pthread_cond_t *cv, pthread_mutex_t *mutex); 等待信号量cv，mutex是互斥锁


## 3.解除在条件变量上的阻塞pthread_cond_signal


int pthread_cond_signal(pthread_cond_t *cv); 成功返回0，发送给阻塞的条件信号


## 4.阻塞直到指定时间pthread_cond_timedwait


int pthread_cond_timedwait(pthread_cond_t *cv, pthread_mutex_t *mp, const structtimespec * abstime);


## 5.释放阻塞的所有线程pthread_cond_broadcast


int pthread_cond_broadcast(pthread_cond_t *cv);


## 6.释放条件变量pthread_cond_destroy


int pthread_cond_destroy(pthread_cond_t *cv);


    #include&lt;iostream&gt;
    #include&lt;pthread.h&gt;
    #include&lt;stdio.h&gt;
    using namespace std;
    
    #define BOUNDARY 5
    
    int task = 10;
    pthread_mutex_t task_mutex;
    pthread_cond_t task_cond;  //条件信号量 处理两个线程间的条件关系
    
    //task&gt;5时thread2处理，反之thread1处理 直到task=0
    
    void*  thread2(void *args){
        pthread_t pid = pthread_self(); //get thread id;
        cout&lt;&lt;"thread2:"&lt;&lt;pid&lt;&lt;endl;
        bool is_signed = false; //标志是否发送过信号
        while(1){
            pthread_mutex_lock(&amp;task_mutex) ; //锁
            if(task&gt;BOUNDARY){
                    cout&lt;&lt;pid&lt;&lt;"thread2 takes task:"&lt;&lt;task&lt;&lt;" in thread "&lt;&lt;*((int *)args)&lt;&lt;endl;
                    --task;
            }else if(!is_signed){
                cout&lt;&lt;pid&lt;&lt;" pthread_cond_signal in thread 2"&lt;&lt;*((int*) args)&lt;&lt;endl;
                pthread_cond_signal(&amp;task_cond); //signal 向thread1发送信号，表明已经&gt;5
                is_signed = true;   //表明信号已发出
            }
            pthread_mutex_unlock(&amp;task_mutex); //解锁
            if(task==0) break;
        }
    }
    
    void*  thread1(void *args){
        pthread_t pid = pthread_self(); //get thread id;
        cout&lt;&lt;"thread1:"&lt;&lt;pid&lt;&lt;endl;
    
        while(1){
            pthread_mutex_lock(&amp;task_mutex) ; //锁
            if(task&gt;BOUNDARY){
                    cout&lt;&lt;pid&lt;&lt;"  pthread_cond_signal in thread 1:"&lt;&lt;*((int *)args)&lt;&lt;endl;
                    pthread_cond_wait(&amp;task_cond,&amp;task_mutex); //等待信号量 收到信号则跳出wait
            }else {
                cout&lt;&lt;pid&lt;&lt;" takes task:"&lt;&lt;task&lt;&lt;" in thread "&lt;&lt;*((int *)args)&lt;&lt;endl;
                --task;
            }
            pthread_mutex_unlock(&amp;task_mutex); //解锁
            if(task==0) break;
        }
    }
    int main(){
        pthread_attr_t attr;
        pthread_attr_init(&amp;attr);
        pthread_attr_setdetachstate(&amp;attr,PTHREAD_CREATE_JOINABLE);
        pthread_mutex_init(&amp;task_mutex,NULL);
        pthread_cond_init(&amp;task_cond,NULL);
    
        pthread_t tid1,tid2;
        int index1 =1 ;
        int ret = pthread_create(&amp;tid1,&amp;attr,thread1,(void*)&amp;index1);
        if(ret!=0){
            cout&lt;&lt;"create pthread error:"&lt;&lt;ret&lt;&lt;endl;
        }
        int index2=2;
       int ret2 = pthread_create(&amp;tid2,&amp;attr,thread2,(void*)&amp;index2);
        if(ret2!=0){
            cout&lt;&lt;"create pthread error:"&lt;&lt;ret2&lt;&lt;endl;
        }
    
        pthread_join(tid1,NULL);
        pthread_join(tid2,NULL);
    
        pthread_attr_destroy(&amp;attr);
        pthread_mutex_destroy(&amp;task_mutex);
        pthread_cond_destroy(&amp;task_cond);
        return 0;
    }
    `

&nbsp;
