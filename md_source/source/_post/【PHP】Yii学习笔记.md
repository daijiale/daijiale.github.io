title: 【PHP】YII学习笔记
date: 2016-12-23 11:25:11
tags:

 - PHP
 - YII

 
categories:

 - 原创博文
 - Web开发笔记
 
---


>PS：之前使用了Symfony2的微框架[Silex](http://silex.sensiolabs.org/)搭建了上一个开源项目[基于Tesseract的图文识别搜索引擎](https://github.com/daijiale/OCR_FontsSearchEngine)的后台，，从而接触了Symfony2框架，经过比较发现，目前市场上比较被开发者所熟知的除了Symfony之外，还有YII、Laravel、thinkPHP等，然而，thinkPHP太不严谨，Laravel对线上机版本兼容太高，不适合中低端机器部署。综合比较之后，选择了YII作为自己第二个PHP框架的学习对象。

<!--more-->


# YII安装

```shell
composer global require "fxp/composer-asset-plugin:~1.1.1"

composer create-project --prefer-dist yiisoft/yii2-app-basic your_project //切换至工程目录下

```

## 验证安装

```shell
php yii serve --port=8888

//http://localhost:8080/下可访问到
//对应http://localhost:8888/your_project/web/

```
# 一个完整的请求周期

- 用户向入口脚本 web/index.php 发起请求。
- 入口脚本加载应用配置 并创建一个应用实例去处理请求。
- 应用通过请求组件 解析请求的路由。
- 应用创建一个控制器实例去处理请求。
- 控制器创建一个操作实例并针对操作执行过滤器。
- 如果任何一个过滤器返回失败，则操作退出。
- 如果所有过滤器都通过，操作将被执行。
- 操作会加载一个数据模型，或许是来自数据库。
- 操作会渲染一个视图，把数据模型提供给它。
- 渲染结果返回给响应组件。
- 响应组件发送渲染结果给用户浏览器。

# Hello World
[Demo](http://www.yiichina.com/doc/guide/2.0/start-hello)

# 结合数据库使用

- 配置一个数据库连接
- 定义一个活动记录类
- 使用活动记录从数据库中查询数据
- 以分页方式在视图中显示数据

[Demo](http://www.yiichina.com/doc/guide/2.0/start-databases)

# 使用Gii生成代码

[Demo](http://www.yiichina.com/doc/guide/2.0/start-gii)




# YII开源项目汇总


- [YII开源社区SNS系统](https://github.com/shi-yang/iisns)

- [基于YII的开源商城系统funshop](https://github.com/funson86/funshop)




# 参考文档

- [YII_官方文档](http://www.yiichina.com/doc/guide/2.0/start-installation)
- [YII_知乎对比](https://www.zhihu.com/question/25023032)
- [深入理解YII2.0](http://www.digpage.com/)
- [YII框架_MAC开发环境-网易云视频学习](http://study.163.com/search.htm?p=yii)
- [YII开源项目](http://www.yii-china.com/site/index.html)
- [YII框架_慕课网](http://www.imooc.com/course/programdetail/pid/55)



