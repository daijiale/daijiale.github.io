title: 【PHP】Composer入门实践
date: 2016-3-8 18:16:11
tags:

 - PHP
 - Composer

categories:

 - 原创博文
 - Web开发笔记

---


这里很有必要和很多初学者提一下，现在Web的开发趋势越来越接近集成化，自动化。Node.js之所以如日中天，npm有很大的功劳，对开发者们起到了十分便捷高效的开发形式。因此，PHP也应运而生了Composer来进行依赖管理。所以无论你以后是要成为专业PHP工程师，还是为了和你未来Team中其他队友“和谐”共事，请掌握之！
<!--more-->

# 简介


Composer是什么？Composer是PHP的依赖管理工具，什么是依赖管理工具呢？它是新出来的一个标准，通过它来管理一个框架一个项目要加载哪些库哪些插件，你可以在一个名为composer.json的文件中定义需要的资源包，通过composer命令可以自动进行下载部署。

Composer 可以看成一个 PHP 的包管理器，类似 Python 的 easy_install 和 pip，Ruby 的 gem，Node.js 的 npm，是一个强大的生态系统。


现在流行的Yii，Laravel等PHP开发框架都推荐使用Composer方式安装框架到本地，它也主要用于本地开发的时候项目的资源管理。




- [简介入门](http://docs.phpcomposer.com/00-intro.html#Globally-on-OSX-via-homebrew)


# Mac下安装

![](http://7xi6qz.com1.z0.glb.clouddn.com/djlBlogcomposer.png)



# 基本使用

[中文文档](http://docs.phpcomposer.com/01-basic-usage.html)

加载Silex Microframework 和Tesseract-OCR PHP两个库示例：

![](http://7xi6qz.com1.z0.glb.clouddn.com/djlBlogphp-composer.png)


# 参考文档
 - [Composer使用五个小技巧](https://segmentfault.com/a/1190000000355928)
 - [Composer中文网](http://www.phpcomposer.com/)
 - [Mac下安装Composer](http://www.tantengvip.com/2015/05/mac-composer/)