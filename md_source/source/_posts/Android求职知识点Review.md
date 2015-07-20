title: Android求职知识点Review
date: 2015-7-20 15:13:09
tags:

 - Android

categories:

 - 原创博文
 - Android开发笔记 

---

<!--more-->

# JAVA

## 1. 举例说明多态和重载区别
     
     ** 重载**
      
 ```     
class A{
void Aa(int a ){....}
void Aa (int a,int b){...} 
}

```

虽然Aa定义了2个 但是因为他们注册的参数不同所以 看作为2个不同的方法

**多态**

```
interface A 
{ 
void Aa();
}
class B implement A
{
void Aa(){ System.out.println("123123"); }
}
class C implement A
{
void Aa(){ System.out.println("abcabc"); }
}
```

多态就是可以理解一个方法被不同实现后 展现不同的效果及状态


## 2. 堆栈

[广义堆栈数据结构](http://zhidao.baidu.com/link?url=nPxYN0s7pcGMl30_IhoQw_K7MykYYQOgiHNIzzeXZt-cLPQAbzJqVOYZvy3aNZayeYpEnHuBA98zXFpJzRa8sq)

[Java中的堆栈](http://blog.csdn.net/chengyingzhilian/article/details/7781858)


## 3. 垃圾回收

[垃圾回收机制](http://blog.csdn.net/zsuguangh/article/details/6429592)

## 4. Final，finally，finalize

[三者区别](http://jingyan.baidu.com/article/597a064363b676312b5243ad.html)

## 5. 序列化反序列化，为什么要有自定义序列化

[序列化和反序列化](http://blog.sina.com.cn/s/blog_8af1069601013ifb.html)

[为什么要自定义序列化](http://blog.csdn.net/linuxandroidwince/article/details/7187778)

## [6. Java的灵活性体现在什么机制上](http://www.cnblogs.com/jqyp/archive/2012/03/29/2423112.html)

## [7. Jdk1.5到1.7有什么新特性](http://www.cnblogs.com/langtianya/p/3757993.html)

## [8. 排序算法](http://blog.csdn.net/hguisu/article/details/7776068)


## [9. 无序数组ab，每个数组有一次循环遍历的机会，找出a有b没有的数字(不能使用外部东西)](http://zhidao.baidu.com/link?url=xNrK6lpmardilOvaN07ekFO1f6GxckKRSbBGA8BpUEGLU_e1zE7lw4aDoFFDvZecbZ-pZvPViB_7HgkO09TsN5KLOhfkOgUZvJXLhxaC8Ua)

## 10. Hashtable和hashmap
[两者区别](http://blog.csdn.net/tianfeng701/article/details/7588091)
[HashMap源码分析](http://www.jb51.net/article/42769.htm)
[HashTable源码分析](http://www.cnblogs.com/caca/p/java_Hashtable.html)
[HashMap和TreeMap区别](http://www.jb51.net/article/32652.htm)

## [11. Hashcode是怎么得到的](http://www.importnew.com/8189.html)
## [12. 线程和进程](http://blog.csdn.net/yaosiming2011/article/details/44280797)
## 13. Sleep和wait区别

sleep指线程被调用时，占着CPU不工作，形象地说明为“占着CPU睡觉”，此时，系统的CPU部分资源被占用，其他线程无法进入，会增加时间限制。
wait指线程处于进入等待状态，形象地说明为“等待使用CPU”，此时线程不占用任何资源，不增加时间限制。
所以
sleep（100L）意思为：占用CPU，线程休眠100毫秒
wait（100L）意思为：不占用CPU，线程等待100毫秒

## 14. 二叉平衡树([红黑树](http://blog.chinaunix.net/uid-26575352-id-3061918.html))，满二叉树
## 15. [Object有哪些基本的方法](http://blog.sina.com.cn/s/blog_6cbdddcb0100m485.html)
## 16. [Io和nio](http://www.jb51.net/article/50621.htm)
## 17. [写一个单例模式的例子](http://blog.csdn.net/jason0539/article/details/23297037)

18. Socket
19. Exception
20. 有向图和无向图什么区别
21. Linux基本命令
22. 数组和链表
23. 深克隆，浅克隆
24. Java的引用类型有哪些，在垃圾回收的时候有什么表现


# 数据库

1.ACID
2. Group by
3. Distinct
4. Where条件的执行顺序是从前往后还是从后往前，还是其他什么顺序
5. [AndroidSQlite](http://www.codeceo.com/article/android-sqlite-course.html)


# 网络

1.你知道什么网络协议，解释一下
2.post和get区别
3.OSI参考模型
4.get的参数是跟在url后面，那post的参数是加在什么地方
5.断点重传
6.PC端的网络连接和移动端有什么区别?
7.定位需要几颗卫星
8.Gps和agps定位有什么区别


# Android

## 1. 横竖屏切换的差别
## 2. [Activity生命周期](http://blog.csdn.net/liuhe688/article/details/6733407)
## 3.[ 什么时候会用到activity生命周期](http://jinguo.iteye.com/blog/691263)
4. Scroll中有listview，出现什么问题，如何解决
5. 消息推送的方法
6. 有一个比较大的图片，如果内存不够加载，怎么办

## 7. Android四大组件 

**Activity 组件介绍**

 Activity是Android中最基本也是最重要的一个组件，它主要负责Android中的页面展示，所有大家看到的Android中的界面，都是Activity。

**ContentProvider组件介绍**

Android中每个应用程序都有自己的内存空间，而且应用程序之间的内存空间是无法相互访问的，这就带来了一个问题，如果几个应用程序之间希望共享一份数据的话，将很难实现。例如，我们手机上有可能有多个短信客户端，但是它们访问的短信数据确是统一份库。Android中通过ContentProvider来实现应用程序间的数据共享。所以应用程序间的数据只有通过ContentProvider来进行分享。

**Service组件介绍**

Service运行在Android的后台，它不和用户直接交互，但是它却能够为用户提供一些服务。例如：后台的音乐播放、后台的任务下载等。当然android系统中大部分与硬件相关的一些功能也都是通过服务来实现的，如电话服务、短信服务和GPS服务等。所以如果当你希望开发的功能是在后台运行的，那么你就应该考虑使用Service实现了。

**BroadcastReceiver组件介绍**

Broadcast是Android中各个应用程序之间传输消息的最基本机制，也是唯一的机制。而我们在应用中就可以通过BroadcastReceiver来截获系统所发出的广播消息，从而获取系统所要传达的消息。例如，接受短信广播，可以实现短信的拦截功能，接受电话广播可以实现电话的黑名单功能等。所以如果你想要实现的功能是通过系统的广播，来实现一些功能，那你就要考虑使用BroadcastReceiver了。

**Intent组件介绍**

之前我们介绍的几个组件，是Android中的基本组件，但是这些组件之间想要进行交互，就一定要使用Intent了。例如，通过Activity去启动另一个Activity，通过Activity去发送广播等。这些都要用到Intent组件。而且很多与系统的功能交互也要使用Intent，所以如果你希望通过一个组件去启动另一个组件的话，就要使用Intent了。

## 8. Android常见数据集合

[List, Map,Set](http://j2eemylove.iteye.com/blog/1195823)