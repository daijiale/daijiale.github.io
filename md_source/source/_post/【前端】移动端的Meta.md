title: 【前端】移动端的Meta
date: 2016-1-30 23:02:01
tags:
 
 - html5

categories:

 - 原创博文
 - Web开发笔记

---


>合理设置 Meta 标签 对PC/手机版网站的搜索引擎优化，PC/手机浏览器的渲染展示都有非常大的帮助。对于桌面平台web布局中大家对meta标签再熟悉不过了，它永远位于 head 元素内部，做SEO的朋友一定对meta有种特殊的感情，这里，博主通过阅读大量博文，结合自身经验，来总结一下列PC端网页和移动端网页中常用的meta标签，并进行逐个分析；meta标签究竟起到哪些神奇的功能呢?
>
<!--more-->

# 什么是Meta标签
 
　　Meta标签给搜索引擎提供了许多关于网页的信息。这些信息都是隐含信息,意味着对于网页自身的访问者是不可见的。
 
　　你可以在网页的元素中发现 标签。在过去,有人曾经问我它是否可以放在网页的，最好不要这样做。如果 标签被放在位置，某些浏览器可能无法识别它们，也就相当于你创建了无效的标签。
 
　　通常情况下， 标签会包含一个name属性，用来设置元数据。元数据的值放在content属性里面。你可以在 标签中使用各种名称/值对，让我们来看看其中的一些。
 
# PC端网页常见的一些Meta标签

## Meta Description
　Meta description标签可能是最有用的标签之一。顾名思义，它会给搜索引擎提供关于这个网页的简短的描述。代码如下:
　
```html
<meta name="”description”" content="”Everything" you="" need="" to="" know="" about="" meta="" tags="" for="" search="" engine="" optimization”="">
```

　　这个标签曾经在搜索排名中占有很大的权重，但随着算法的不断更新升级，它的地位也逐渐降低。它虽然不会提高网站排名，但是，因为它会被用在搜索引擎的结果页，所以依然有用。
　　这也就意味着它仍然可以提高你的网页点击率。毕竟，当用户搜索的关键词与之相匹配时，会以粗体显示突出显示。这就是为什么一个好的页面说明 (利用关键字的) 可以显示更多与用户相关的信息，进而提高了点击率。推荐的description长度为160 个字符。
　　但是如果你没有使用description标签或者description标签为空时，会发生什么呢?搜索引擎仍会在搜索结果页显示出自己创键的一小段文字。大多数的结果都不是用户需要的，也就意味着你将失去用户点击网页的机会。
 
## Meta Robots
 
　　我们在之前的教程中已经接触过Meta robots标签。如果你没有机会回去阅读它，这里有一段简短的介绍：
 
　　Meta robots标签管理着搜索引擎是否可以进入网页，你可以用它来允许或不允许搜索引擎来获取你的网页、进入你网页中的子链接或对你的网页存档。例如：
```html
<meta name="”robots”" content="”noindex," nofollow”="">
```
       这个meta 标签告诉搜索引擎不要获取网页，并且阻止其进入链接。如果你不小心使用了两个矛盾的术语 (例如noindex和index)，谷歌会选择最具限制性的选项。
       为什么这个标签会对搜索引擎优化(SEO)起作用呢?首先，它可以防止对拷贝内容的冗余抓取，例如页面的打印版页面。它也可能会对那些内容不完整的页面或者而存在私密信息的网页起作用。
 
##Title
 
　　专业的讲，title标签不是meta标签，但他们都放在相同位置。我之所以把title标签放在这里是因为它对搜索引擎优化很重要。
 
　　在所有的HTML文档中，title标签都是不可缺少的。它定义了整个文档的标题.标题通常会显示在两个不同的地方;浏览器的头部标签和搜索结果页。这就意味着title标签在点击率(CTR)和排名上有很重要的影响。一个好的标题应该包含关键字，而且最好放在标题的开头部分。请记住，那些匹配到用户搜索的关键字会以粗体显示。另一件你应该牢记在心的事情就是标题的长度。谷歌会限制标题为70个字符，所以偶尔你可能需要书写一个合适的标题。Dan Shure发表过一篇很不错的关于标题的文章，叫are your titles irresistibly click worthy and viral?，包含了很多有意思的知识点。
 
　　
#PC端网页一些不经常使用的Meta标签
 
## Meta Content Type (charset)
      `meta content type`标签被用来声明网页的字符编码，为了防止浏览器产生编码问题最好加上这个属性。但是它不会影响搜索排名或点击率(CTR)。

 
## Meta Keywords
 
　　这个标签在过去很重要，但是现在却没什么价值了。现在没有一个主流的搜索引擎使用`meta keywords`来判断网页的内容了。
   　在`meta keywords`标签里面，你可以存储几个关于网页内容的关键字。然而，它却不会提高你的排名。如果你想要实现它(尽管我不知道你为什么这样做)你可以用如下代码：
```html
<meta name="”keywords”" content="”meta" engine="" optimization”="" tags,search="">
```
## Meta Language
 
　　这个标签之前是用来声明网页语言的。可以告知屏幕阅读器和其它文本处理器他们正在处理的语言以便更好的工作。这就是为什么`meta language`的content声明为什么可以为fr。
```html
<meta content="fr" http-equiv="content-language">
```

##Notranslate
　　有时，Google在结果页面会提供一个翻译链接，但有时候你不希望出现这个链接，你可以添加这样一个meta标签：
```html
<meta name="”google”" content="”notranslate”">
```

## Refresh
 
　　使用这个meta标签你可以控制浏览器在一段时间之后自动刷新。举例说明，下面的代码表示每隔30秒网页自动更新：
```html
<meta content="”30”" http-equiv="”refresh”">
```
　　你也可以在刷新之后跳转到另外一个页面，看看下面这个例子：
```html
<meta content="”30;URL=’http://website.com’”" http-equiv="”refresh”">
```
　　W3C是不推荐使用这个标签的，因为它会令用户产生迷惑。另外，它对搜索排名没有任何影响。
 
# PC端网页中的Meta总结
 
　　简单的说，有三个meta标签，你应该关注一下：description、robots、title(经常被视为是，但专业来讲不是).
　　description标签被用来显示更多有关网页内容的信息，搜索引擎也会在搜索引擎结果页面(SERP)使用它。robots标签用来阻止搜索引擎获取拷贝页面、私密页面和未完成的页面。最后，最重要的title标签，控制它在70个字符以下，并在其中使用关键词。
 　keywords标签的时代已经过去，最好不在使用它。其他一些比较重要的标签(有关搜索引擎优化)：language、content、refresh、nontranslate。
**相关的meta设置**：
　　
```html
<meta charset="UTF-8">
<meta http-equiv="refresh" content="5;url=" />
<link rel="copyright" href="copyright.html" 　/>
<meta http-equiv="X-UA-Compatible" content="IE=edge" />
<meta name="viewport" content="width=device-width, initial-scale=1" />
<meta name="description" content="150 words" />
<meta name="keywords" content="your tags" />
<!--
all：文件将被检索，且页面上的链接可以被查询;
none：文件将不被检索，且页面上的链接不可以被查询;
index：文件将被检索;
follow：页面上的链接可以被查询;
noindex：文件将不被检索;
nofollow：页面上的链接不可以被查询。
-->
<meta name="robots" content="index,follow" />
<meta name="author" content="author name" />
<meta name="google" content="index,follow" />
<meta name="googlebot" content="index,follow" />
<meta name="verify" content="index,follow" />
<!-- 启用 WebApp 全屏模式 -->
<meta name="apple-mobile-web-app-capable" content="yes" />
<!-- 隐藏状态栏/设置状态栏颜色：只有在开启WebApp全屏模式时才生效。content的值为default | black | black-translucent 。 -->
<meta name="apple-mobile-web-app-status-bar-style" content="black-translucent" />
<!-- 添加到主屏后的标题 -->
<meta name="apple-mobile-web-app-title" content="标题">
<!-- 忽略数字自动识别为电话号码 -->
<meta content="telephone=no" name="format-detection" />
<!-- 忽略识别邮箱 -->
<meta content="email=no" name="format-detection" />
<meta name="apple-itunes-app" content="app-id=myAppStoreID, affiliate-data=myAffiliateData, app-argument=myURL" />
<!-- 添加智能 App 广告条 Smart App Banner：告诉浏览器这个网站对应的app，并在页面上显示下载banner:https://developer.apple.com/library/ios/documentation/AppleApplications/Reference/
SafariWebContent/PromotingAppswithAppBanners/PromotingAppswithAppBanners.html -->
<!-- 针对手持设备优化，主要是针对一些老的不识别viewport的浏览器，比如黑莓 -->
<meta name="HandheldFriendly" content="true">
<!-- 微软的老式浏览器 -->
<meta name="MobileOptimized" content="320">
<!-- uc强制竖屏 -->
<meta name="screen-orientation" content="portrait">
<!-- QQ强制竖屏 -->
<meta name="x5-orientation" content="portrait">
<!-- UC强制全屏 -->
<meta name="full-screen" content="yes">
<!-- QQ强制全屏 -->
<meta name="x5-fullscreen" content="true">
<!-- UC应用模式 -->
<meta name="browsermode" content="application">
<!-- QQ应用模式 -->
<meta name="x5-page-mode" content="app">
<!-- windows phone 点击无高光 -->
<meta name="msapplication-tap-highlight" content="no">
```


# 移动端Meta 之 viewport
 
　　说到移动平台meta标签，那就不得不说一下viewport了，那么什么是viewport呢?
　　viewport即可视区域，对于桌面浏览器而言，viewport指的就是除去所有工具栏、状态栏、滚动条等等之后用于看网页的区域。对于传统WEB页面来说，980的宽度在iphone上显示是很正常的，也是满屏的，但对于webapp而言，可能就有点问题了，在iphone上我们的webapp在竖屏下通常宽度都是320，这时我们320页面在iphone上显示成啥效果呢?有人可能认为iPhone不是320的宽度么，感觉应该是满屏的吧，事实呢?我们来看一如下布局在iPhone上的显示情况：
```html
<style type="text/css"> 
div,body{ 
padding:0; 
margin:0; 
} 
body{ 
padding-top:100px; 
color:#fff; 
} 
div{ 
width:320px; 
height:100px; 
margin:0 auto; 
background:#000; 
text-align:center; 
font:30px/100px Arial; 
} 
</style> 
<div> 
AppUE 
</div> 
```

因此我们必须改变viewport，我们就有如下几种属性值可以设置：
 - width: viewport 的宽度 (范围从 200 到 10,000 ，默认为 980 像素 )
 - height: viewport 的高度 (范围从 223 到 10,000 )
 - initial-scale: 初始的缩放比例 (范围从>0到 10 )
 - minimum-scale: 允许用户缩放到的最小比例
 - maximum-scale: 允许用户缩放到的最大比例
 - user-scalable: 用户是否可以手动缩放
 
　　对于这些属性，我们可以设置其中的一个或者多个，并不需要你同时都设置，iPhone 会根据你设置的属性自动推算其他属性值，而非直接采用默认值。
 
　　如果你把initial-scale=1 ，那么 width 和 height在竖屏时自动为320X356 (不是320x480 因为地址栏等都占据空间 )，横屏时自动为 480x208。类似地 ，如果你仅仅设置了 width ，就会自动推算出initial-scale 以及height。例如你设置了 width=320 ，竖屏时 initial-scale 就是 1 ，横屏时则变成 1.5 了。 那么到底这些设置如何让 Safari 知道 ?其实很简单 ，就一个 meta ，形如 ：
　　
```html
<meta name="”viewport”" content="”width=device-width;" user-scalable="0;”" maximum-scale="1.0;" initial-scale="1.0;">
```
好了，我们就可以按全屏来布局我们的页面了，不用再担心页面显示的很小了!。


#移动端Meta 之 format-detection

你明明写的一串数字没加链接样式，而iPhone会自动把你这个文字加链接样式、并且点击这个数字还会自动拨号!想去掉这个拨号链接该如何操作呢?这时我们的meta又该大显神通了，代码如下：
```html
<meta name="”format-detection”" content="”telephone=no”">
```
telephone=no就禁止了把数字转化为拨号链接!telephone=yes就开启了把数字转化为拨号链接，要开启转化功能，这个meta就不用写了,在默认是情况下就是开启!

# 移动端Meta 之 apple-mobile-web-app-capable

```html
<meta name="”apple-mobile-web-app-capable”" content="”yes”"> 
```
这meta的作用就是删除默认的苹果工具栏和菜单栏。content有两个值”yes”和”no”,当我们需要显示工具栏和菜单栏时，这个行meta就不用加了，默认就是显示。

#移动端Meta 之 apple-mobile-web-app-status-bar-style

```html
<meta name="”apple-mobile-web-app-status-bar-style”" content="”default”"> 
<meta name="”apple-mobile-web-app-status-bar-style”" content="”black”"> 
<meta name="”apple-mobile-web-app-status-bar-style”" content="”black-translucent”">
```

作用是控制状态栏显示样式
- status-bar-style:black 
- status-bar-style:black-translucent


# 移动端网站中一般需要添加哪几种 Meta 标签：
## viewport 
viewport 几乎已经是公认的标准了，最初是由苹果公司创建，用于 iPhone 上面的移动版 Safari，由于 iPhone 的大卖，大部分其他移动浏览器都接受，比如 Opera Mobile, iPhone, Android, Iris, IE, BlackBerry, Obigo, Firefox。
　　最基本的例子，在移动上使站点全屏宽度：
　　
```html
<meta name="viewport" content="width=device-width, initial-scale=1.0">
```

## HandheldFriendly

这个标签和下面介绍的 MobileOptimized 是功能机时代的事实上标签。
 
　　HandheldFriendly 标签最早在 AvantGo 浏览用来标示移动内容的，后来变成一个通用的标准用来标示移动站点，但是不知道这个标签的支持情况。
```html
<meta name="HandheldFriendly" content="true">
```
　　
## MobileOptimized
　　这是一个 Windows 专有的 meta 标签，最终成为用于识别移动内容的另一种方法，但是该标签的缺点是，特定的宽度必须给出，再次，这个标签的支持情况也是未知的：
```html
<meta name="MobileOptimized" content="320"/>
```
# 参考文献

- [移动前端不得不了解的html5 head 头标签](http://www.css88.com/archives/5480/comment-page-1)
- [在移动网站中需要添加哪几种 Meta 标签？](http://www.bitscn.com/school/jzjq/201411/407135.html)
