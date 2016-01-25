title: 【前端】FIS3应用小记
date: 2016-1-28 16:30:09
tags:
 
 - Node.js
 - Javascript
 - 前端自动化工具

categories:

 - 原创博文
 - Web开发笔记

---



>接触FIS是在2014年11月左右，当时大二，在准备百度实习面试的时候网上迷路了，偶遇了FIS的官网，当时用了一下，觉得是一个基于Node.js的前端自动化解决方案，没太在意...后面在百度遇见了一些前端的小伙伴，才明白FIS的潜能，现在不管是自己的项目，还是以后公司的前端项目，都快离不来TA了，之前听一个学弟说最近他也在使用，并且存在不少问题，所以想写篇博文，给需要的小伙伴们。PS：我自己用的是MacOS系统，博文所述的FIS版本采用最新的FIS3。
>
<!--more-->

# 【前端】FIS3应用小记

## FIS3
FIS3：面向前端的工程构建工具，一套来自度厂的比较完整的前端开发解决方案，类似于[`grunt`](http://www.gruntjs.net/)。


## FIS3安装
- 请戳：[安装教程](http://fis.baidu.com/fis3/docs/beginning/install.html)。
- 注意：UNIX系统的终端在输入安装命令时请+`sudo`。
- NPM被墙，慢出翔，请使用国内镜像安装。
- 安装遇见Bug不慌：[常见问题解决方案](https://github.com/fex-team/fis/issues/565)。
- 安装成功后记得`fis -v`check一下。

## FIS3构建项目
### 资源定位
如下图，切换至项目`fis-conf.js`所在根目录下，键入`fis release -d <path>`,构建发布到项目目录的`<path>`目录下（可以是子目录，也可以是父目录，e.g.：`./output_son`,`../output_father`）。

![](http://7xi6qz.com1.z0.glb.clouddn.com/djlBlogfis1.png)

### 配置文件
默认配置文件为 `fis-conf.js`，FIS3 编译的整个流程都是通过配置来控制的：
 
 - [配置指南（Demo）](http://fis.baidu.com/fis3/docs/api/config.html)
 
 - [常用配置（套路）](http://fis.baidu.com/fis3/docs/api/config-props.html)
 
 - [配置属性（API）](http://fis.baidu.com/fis3/docs/api/config-props.html)
 
###  文件指纹
文件指纹：唯一标识一个文件。在开启强缓存的情况下，如果文件的 URL 不发生变化，无法刷新浏览器缓存。一般都需要通过一些手段来强刷缓存，一种方式是添加时间戳，每次上线更新文件，给这个资源文件的 URL 添加上时间戳。
```html
<img src="a.png?t=12012121">
```
而 FIS3 选择的是添加 MD5 戳，直接修改文件的 URL，而不是在其后添加 query。
对 js、css、png 图片引用 URL 添加 md5 戳，配置如下；

```javascript
//清除其他配置，只剩下如下配置
fis.match('*.{js,css,png}', {
  useHash: true
});
```	

### 压缩资源
为了减少资源网络传输的大小，通过压缩器对 js、css、图片进行压缩是一直以来前端工程优化的选择。在 FIS3 中这个过程非常简单，通过给文件配置压缩器即可。

```javascript
// 清除其他配置，只保留如下配置
fis.match('*.js', {
  // fis-optimizer-uglify-js 插件进行压缩，已内置
  optimizer: fis.plugin('uglify-js')
});
fis.match('*.css', {
  // fis-optimizer-clean-css 插件进行压缩，已内置
  optimizer: fis.plugin('clean-css')
});
fis.match('*.png', {
  // fis-optimizer-png-compressor 插件进行压缩，已内置
  optimizer: fis.plugin('png-compressor')
});
```


### 图片合并
压缩了静态资源，我们还可以对图片进行CssSprite合并，来减少请求数量。
FIS3 提供了比较简易、使用方便的图片合并工具。通过配置即可调用此工具并对资源进行合并。
FIS3 构建会对 CSS 中，路径带 ?__sprite 的图片进行合并。为了节省编译的时间，分配到 useSprite: true 的 CSS 文件才会被处理。

```html
li.list-1::before {
  background-image: url('./img/list-1.png?__sprite');
}
li.list-2::before {
  background-image: url('./img/list-2.png?__sprite');
}
```
启用 fis-spriter-csssprites 插件
```javascript
// 启用 fis-spriter-csssprites 插件
fis.match('::package', {
  spriter: fis.plugin('csssprites')
})
// 对 CSS 进行图片合并
fis.match('*.css', {
  // 给匹配到的文件分配属性 `useSprite`
  useSprite: true
});
```
[CssSprites 详细配置参见](https://github.com/fex-team/fis-spriter-csssprites)

### 功能组合

```javascript

// 加 md5
// fis.match('*.{js,css,png}', {
//   useHash: true
// });

// 启用 fis-spriter-csssprites 插件
// fis.match('::package', {
//   spriter: fis.plugin('csssprites')
// })

// 对 CSS 进行图片合并
// fis.match('*.css', {
//   // 给匹配到的文件分配属性 `useSprite`
//   useSprite: true
// });

// fis.match('*.js', {
//   // fis-optimizer-uglify-js 插件进行压缩，已内置
//   optimizer: fis.plugin('uglify-js')
// });

// fis.match('*.css', {
//   // fis-optimizer-clean-css 插件进行压缩，已内置
//   optimizer: fis.plugin('clean-css')
// });

// fis.match('*.png', {
//   // fis-optimizer-png-compressor 插件进行压缩，已内置
//   optimizer: fis.plugin('png-compressor')
// });
// 
 
// fis.media() 接口提供多种状态功能，比如有些配置是仅供开发环境下使用，有些则是仅供生产环境使用的。
// fis.media() 可以使配置文件变为多份（多个状态，一个状态一份配置）。
// fis3 release rd push 到 RD 的远端机器上
// fis3 release qa push 到 QA 的远端机器上
// fis.media('rd').match('*', {
//   deploy: fis.plugin('http-push', {
//     receiver: 'http://remote-rd-host/receiver.php'
//   })
// });

// fis.media('qa').match('*', {
//   deploy: fis.plugin('http-push', {
//     receiver: 'http://remote-qa-host/receiver.php'
//   })
// });
// 



//启用 fis-spriter-csssprites 插件
fis.match('::package', {
  spriter: fis.plugin('csssprites')
});

fis.match('*.js',{
	useHash:false,
	optimizer: fis.plugin('uglify-js')
});

fis.match('*.css',{
	useHash:false,
	optimizer: fis.plugin('clean-css'),
	// 对 CSS 进行图片合并
	// 给匹配到的文件分配属性 `useSprite`
	useSprite: true
});

fis.match('*.png',{
	useHash:false,
	optimizer: fis.plugin('png-compressor')
});

// 可能有时候开发的时候不需要压缩、合并图片、也不需要 hash。那么给上面配置追加如下配置；
// fis release debug 启用 media debug 的配置，覆盖上面的配置，把诸多功能关掉。

fis.media('debug').match('*.{js,css,png}', {
  useHash: false,
  useSprite: false,
  optimizer: null
})

```

## FIS3调试


## FIS3使用

## 参考文献
 - [FIS3官网](http://fis.baidu.com/fis3/docs/beginning/intro.html)
 
 - [FIS使用初级视频操作教程](http://www.imooc.com/learn/220)
 
 - [FIS3内置语法](http://fis.baidu.com/fis3/docs/user-dev/extlang.html) 
 - [FIS3接口文档](http://fis.baidu.com/fis3/docs/api/command.html)