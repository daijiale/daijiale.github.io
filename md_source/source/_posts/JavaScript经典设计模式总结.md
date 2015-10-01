title: JavaScript经典设计模式总结
date: 2015-6-9 11:29:31
tags:

 - Javascript

categories:

 - 原创博文
 - Web开发日记

---


# 前言 #

> 在百度即将离职的最后一个月里，我被派往新项目担任FE的角色，借此机会也重新review一下大一大二的老本行——Web前端开发。这篇Blog是对自己所学2年JS的一个设计模式总结，也是实际编码中经常用到的很多经典Case，希望能帮助到更多JS开发者，也给那些还活跃在 `JS API` 层的同学打开一个新的学习思路。
PS：本篇Blog侧重点在于JS的设计模式，如果只是期望了解JS语法，请直接戳[W3C传送门](http://www.w3school.com.cn/js/index.asp)！
<!--more-->


# 一、JS基础 #

## 动态类型语言和鸭子类型

## this、call、apply
Javascript 的`this`总是指向一个对象，而具体指向哪个对象是在运行时基于函数的执行环境动态绑定的，而不是函数被声明时的环境。

### this的指向
除去不常用的`with`和`eval`情况。

1、作为对象的方法调用
当函数作为对象的方法被调用时候，this指向该对象：
```
var obj = {


}

```

2、


#最后汇总一些比较好的技术文章和其他资料
 - [javascript异步编程的4种方法](http://www.w3cfuns.com/blog-5465288-5408787.html)