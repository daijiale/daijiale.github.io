title: 【前端】基于百度ODP的Web前端开发
date: 2015-6-10 11:03:09
tags:
 
 - Amaze UI
 - PHP
 - html5

categories:

 - 原创博文
 - Web开发笔记

---


> ODP，即Online Develop Platform，是为百度Web业务提供完善且高度集成的基础环境、框架、基础库和工具，统一规范模式。

> 个人认为，这套PHP开发框架和TP类似，但是安全性和效率性方便远胜于TP
> 
> 由于新业务的需求，转向FE的角色，这几天为此调研了一下基于ODP的Web前端开发。

<!-- more-->

# ODP的安装和部署 #

[请参考内部文档](http://man.baidu.com/inf/odp/tutorial/#ODP安装)


# ODP文件环境 #
直接切到odp文件夹下：

```

[chenyijie@cq01-ksarch-rdtest00.vm odp] $ pwd
/home/chenyijie/odp
[chenyijie@cq01-ksarch-rdtest00.vm odp] $ ls
app conf data install log php template webroot webserver

```

## app ##

用于放置产品线业务`app`代码，类似于TP中的`application目录`

## conf ##
`conf目录`中包含了四个文件夹，十个文件，为配置目录，用于存放组件和`app`的配置。类似于TP中的`config.yaml`

内含文件名称以及用途简介：

 - app	文件夹，用于存放app相关的配置文件
 - db	文件夹，存放数据库相关的配置文件：cluster.conf global.conf
 - i18n	文件夹，存放国际化相关的配置文件：feature.conf locale.conf
 - ral	文件夹，存放RAL（资源访问层）相关的配置文件，内容较复杂，见后续章节介绍
 - idc.conf	Conf组件的分机房配置与RAL组件的配置相关
 - init.conf	init组件的配置文件
 - log.conf	配置log目录下的日志如何打印的配置文件
 - saf.conf	SAF框架的配置文件
 - smarty.conf	Smarty模板的配置文件
 - passport.conf	Passport服务的配置文件
 - pcheck.conf	pcheck静态代码检查工具的配置文件
 - phptest.conf	phptest单元测试框架组件的配置文件
 - layerproxy.conf	layerproxy组件的配置文件

## data ##

  本地数据目录，用来放置组件和app生成的本机数据、缓存，类似于TP的`data`

## install ##

为组件安装信息存储目录，OCM命令读取的即为该目录信息。类似于TP中的钩子目录：`plugins`

## log ##

生产日志目录，对调试信息的查找十分便捷。

## php ##
是ODP环境包含的php安装后所在的目录，可以在此目录中查看php安装的扩展、以及对php解析的开启与关闭操作。php目录中包含的phplib目录为公共库目录，存放ODP和产品线的公共库，bin目录为PHP程序目录，存放PHP和ODP的工具程序。

## template ##
用于存放php的页面模板，ODP环境支持火麒麟与smarty模板技术，因此template目录下可以存放以.tpl为后缀的文件。类似于TP中的`tpl`


## webroot ##
webroot目录为默认的web文档目录，用于存放静态文件，以及每个app的index.php。

## webserver ##
webserver目录为webserver的安装目录，当前ODP支持lighttpd以及nginx两种服务器。



# 在Template下进行前端模版开发 #

研究了一下电视游戏的前端代码，发现和TP比较类似：

**举例：**

TP的做法：

	Tpl
	- uestc_club
		- public
		   - CSS
		   - images
		   - js
		   - nav.html
		   - head.html
		   - footer.html
		   - scripts.html
		- User
		- portal
			-index.html
			- list.html
			- article.html
		- config.html
		- error.html
		- success.html
		- jump.html


ODP的做法：

	template
	- tvgame
		 - css
		 - download
		 - feedback
		 - file
			 - .apk
		 - home
		 	- index.tpl
		 - images
		 - js
		 - mob
		 	- about.tpl
		 	- aboutNew.tpl
		 	- help.tpl
		 - passport

**前端主要区别：**

 - `TP`一般用的`vo`渲染前端页的数据。
 - `ODP`一般用`Smarty`渲染`tpl`文件生成前端。


（经`大卓`透露：`Smarty`对`OCM`——ODP封装了docker技术后的产物，兼容性不好，有大坑未解决，所以暂时不用，直接采取插入php变量进行数据交互）

# H5+AmazeUI 实现移动端WebView#

## html5表单标签新特性： ##

[Input 类型](http://www.w3school.com.cn/html5/html_5_form_input_types.asp)

[表单元素](http://www.w3school.com.cn/html5/html_5_form_attributes.asp)

## AmazeUI ##

### 为什么不用bootstrap？ ###

 - bootstrap只是对HTML增加了CSS进行美化和响应式，而amaze ui则在bootstrap美化的基础上，主要增加了[常见的JS动态](http://amazeui.org/javascript/slider)，以及更多样式。
 - 支持国产吧


### Demo演示 ###

[H5+AmazeUI实现的“车生活_Demo”](http://2.daijialewebdesign.sinaapp.com/MapCar/BaiduMapCar.html)

![](http://7xi6qz.com1.z0.glb.clouddn.com/github_myblogBaiduMapCar1.png)
![](http://7xi6qz.com1.z0.glb.clouddn.com/github_myblogBaiduMapCar2.png)
![](http://7xi6qz.com1.z0.glb.clouddn.com/github_myblogBaiduMapCar3.png)
![](http://7xi6qz.com1.z0.glb.clouddn.com/github_myblogBaiduMapCar4.png)
# 移动端内核兼容性分析 #

## Android WebView兼容性 ##

**Android WebView内核：**

 - 在Android 4.4以下(不包含4.4)系统WebView底层实现是采用[WebKit](http://www.webkit.org/)内核。
 - 在Android 4.4及其以上Google 采用了[chromium](http://www.chromium.org/)作为系统WebView的底层内核支持。


WebKit for WebView VS Chromium for WebView性能比对（测试环境 小米2. CM Browser. Android 4.1.1 VS 4.4.3）：

<table border="1">
<tr>
<td></td>
<td>Webkit for Webview</td>
<td>Chromium for Webview</td>
<td>备注</td>
</tr>
<tr>
<td>HTML5</td>
<td>278</td>
<td>434</td>
<td><a href="http://html5test.com/">http://html5test.com/</a></td>
</tr>
<tr>
<td>远程调试</td>
<td>不支持</td>
<td>支持</td>
<td>不支持</td>
</tr>
<tr>
<td>内存占用</td>
<td>小</td>
<td>大</td>
<td>相差20-30M左右</td>
</tr>
<tr>
<td>WebAudio</td>
<td>不支持</td>
<td>支持</td>
<td>Android5.0以上支持</td>
</tr>
<tr>
<td>WebGL</td>
<td>不支持</td>
<td>支持</td>
<td>Android5.0以上支持</td>
</tr>
<tr>
<td>WebRTC</td>
<td>不支持</td>
<td>支持</td>
<td>Android5.0以上支持</td>
</tr>
</table>


**注意点：**

 - 通过webview调起本地服务的时候需要在`activity`的`onActivityResult`里做处理 ，`setWebViewClient`和`setWebChromeClient`去监听一下`WebView`的回调。

 - 主流手机对HTML5的支持都是很不错的，webview一般也都是用的硬加速，不过Android和IOS相比还是比较卡。


## IOS WebView兼容性 ##

 iOS 8拥有4个Web内核引擎，当然，也意味着兼容性和bug都是有差异的。 

 - 1. Safari
 - 2. Web.app (使用full-screen 桌面应用)
 - 3. UIWebView (老)
 - 4. WKWebView（新）

这些都是支持HTML5的。