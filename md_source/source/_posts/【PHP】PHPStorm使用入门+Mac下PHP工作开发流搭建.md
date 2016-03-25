title: 【PHP】PhpStorm使用入门+Mac下PHP工作开发流搭建
date: 2015-12-25 11:56:11
tags:

 - PHP
 - PhpStorm

categories:

 - 原创博文
 - Web开发笔记

---


# 前言 #

> 换了RetineMacPro2015之后，还没怎么做PHP的大型项目，之前很多小的Web后端都使用的MAMP来进行PHP开发，因为项目小，基本都不要太多调试，所以MAMP还挺满足需求的，但是也受够了只能通过`echo`,`print`来行调试的窘境。所以有了这篇博客，也为了帮助以后有需要的童鞋们可以少走弯路。
<!--more-->


# Brew安装 #
>玩过ubuntu的童鞋应该都很清楚：在使用ubuntu的过程中apt-get是一个及其重要的工具，负责了几乎所有软件的安装、卸载、更新工作。使用简单但功能强大。如果使用Mac OS，开启`终端`（觉得终端也不好用的，可以看看这个——[如何善加利用 Mac 下的 Terminal ？](https://www.zhihu.com/question/29442452),博主现在用的是[Zsh](https://github.com/robbyrussell/oh-my-zsh)），之后发现一些好用的命令行工具都没有，比如wget或unrar，这很郁闷。然而，令人兴奋的是：`Brew`是`apt-get`的一个替代品，这也是为什么我要在这篇博客里先强调要安装`Brew`的原因。

- [官网安装说明](http://brew.sh/index_zh-cn.html)






# Phpstorm安装 #

为什么选用`Phpstorm`，因为博主有新版强迫症（Linux下还是建议直接`Vim`）。大家可以结合自己的调研和猿性来选择最称手的IDE，下面有篇博文参考：[PHP集成开发环境对比](http://www.cnblogs.com/lishiyun19/p/4297791.html)。

- [Phpstorm10.0.2 for OSX 以及破解方法打包下载](http://pan.baidu.com/s/1gdWJJXL)


注意：因为Phpstorm执行调试程序需要指定Php版本，而Mac系统自带安装的PHP是5.5.x版本，位于usr/bin/php目录下，此目录并无php.ini以及更多php扩展库文件，所以需要我们用到之前安装的`Brew`来进行自定义的PHP版本下载，默认下载路径到usr/local/etc/php/下。


# Brew 安装PHP和PHP扩展库


打开终端：
参看如下步骤：[安装指南](http://www.phperz.com/article/14/0819/18934.html)

注意：因为brew版本更新原因：可能会出现如下问题：

```
Error: Formulae found in multiple taps:
 * homebrew/php/php53
 * josegonzalez/php/php53
Please use the fully-qualified name e.g. homebrew/php/php53 to refer the formula.
```

博主的解决方案是：

```
brew untap josegonzalez/php
brew tap --repair
brew update

```

![](http://7xi6qz.com1.z0.glb.clouddn.com/djlBlogbrew_fix_bug.png)




# PhpStorm构建Xdebug进行调试

- 首先在Phpstorm中指定好刚下载好的自定义Php版本路径（含有php.int和其他扩展库）。
- [参考教程](http://www.cnblogs.com/lishiyun19/p/4470086.html)，配置Xdebug。


# PhpStorm构建MAMP服务器测试环境

- [参考教程](http://blog.sina.com.cn/s/blog_a30629730102v6sc.html) 配置，成功后，测试如下图所示：
![](http://7xi6qz.com1.z0.glb.clouddn.com/djlBlog_phpstormtest.png)









