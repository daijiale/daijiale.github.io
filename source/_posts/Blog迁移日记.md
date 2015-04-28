title: 从“主机搬家、第五次个人主站更新、第三次博客技术架构迁移”说起
date: 2015-4-2 23:30:09

tags:

- Node.js
- Hexo
- Blog
- 个人故事

categories:

- 原创博文
- Web开发日记


---

**以下所指“更新”：均为Blog技术更新，非文章更新**
#第一次更新_个人Blog
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;说起自己的第一个Blog，要从小学刚开通QQ空间开始，具体几年级已经记不清了，那个时候刚甩掉纸质日记本，然后就迈入网络日志的深坑。以前倒是产出了不少优质日志，不过都是些多愁善感的随便，至今都已经封存进_私密日日记_中(如下图)：![](http://7xi6qz.com1.z0.glb.clouddn.com/djlblogpic_qqzonesec.PNG)
<!--more-->
#第二次更新_个人Blog
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;后面高中开始玩人人，Blog也顺势搬到人人。。。现在想来这搬迁史也很简单——围绕社交圈，毕竟Blog写出来是给人看的，而QQzone和人人都是一个很好的社交载体。当然，如今的人人，我早已放弃，一个月都不登一次的这种，blog数据也全部清除了，高中写的都是些没营养的东西，个人认为没有存在的价值。
最后留张图纪念一下曾经的人人帐号：![](http://7xi6qz.com1.z0.glb.clouddn.com/djlblogpicrenren.PNG)

#第三次更新_个人Blog
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;后面因为大学专业性质原因，再加上由于个人兴趣先接触的知识是Web开发，所以打算通过自己编写代码，建立带有自己专属域名的独立Blog全站（其实是不知道做什么内容。。。就以Blog为题材了，而且身为一个程序猿，Blog是撑脸面的东西）。正如很多刚入门的小白一样：最初选用的技术框架是 `PHP+MySQL`，边学边写，坚持了一段时间后发现根本没法拿出手，在人生导师 `@阔空晴云`（亦师亦友的关系，大学同学，年级传奇....我Blog友情链接有他的Blog..但是，为保留他的个人隐私，我就不透露__林志豪__的__真名__了）的建议下，
后来选用了知名度很大的 `wordpress` 框架去快速开发能拿得出手的Blog，那也是我第一次接触框架类的东西，在那之前都是直接C++和C做底层开发，js和html也都是从新建文件开始码起，-->你懂得...从大众空间到自己独立虚拟主机空间，也算是第一次技术迁移了！网站是托管在人生导师的虚拟主机上，记得刚拍下[www.daijiale.cn](http://www.daijiale.cn)个人域名，挂到上线的时候是这样子的:
![](http://7xi6qz.com1.z0.glb.clouddn.com/github_daijialewebsiteworkshop.png)

嗯，对于一个大一上刚学会做站点的小虾开发者来说，还是感觉Geek满满的。

#第四次更新_个人Blog
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;自上一次大更新以后的两年多里，都没有什么大的起伏（主要是太懒，或者忙其他项目去了），直到后面阴差阳错侥幸获得了这个证书：![](http://7xi6qz.com1.z0.glb.clouddn.com/SAEDeveloper.jpg),新浪每月给我SAE上打的云豆工单根本用都用不完了，so，我打算把Blog全部迁移到 `SAE上` （毕竟老放在人生导师那也不太好）。顺便更新了下Blog的界面和交互，增加了首页响应式效果（方便宣传嘛，嘿嘿）,自己设计和编码水平的提高也让每次迭代后的Blog页面颜值更高：
![](http://7xi6qz.com1.z0.glb.clouddn.com/daijialeweb_personalsite.png)



#第五次更新_个人Blog
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;这次改动比较大，也就是目前大家看到的这个站点：[http://daijiale.github.io/](http://daijiale.github.io/)（域名换了，因为托管原因，把原来的[www.daijiale.cn](http://www.daijiale.cn)改成了简历页和入口页，其实也挺不错，性质不一样，刚好趁着这次技术整改实现了分布式控制），整个技术体系和空间都搬家+大改了，这次的技术架构没有采用市面上被大家广泛使用的常见技术，__没有服务器脚本语言的参合，没有数据库，没有进行数据存储优化__，简而言之就是，妈的！这逼网站**居然没有后台**（其实我心里一开始是拒绝的）。是的，你没听错，这特么就是个伪静态Blog，但是...这丝毫不影响ta的逼格大增,so let’s see it：

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;技术框架选用的是 `Hexo+Node.js+ejs+bootstrap+markdown+GithubPage`，为什么会采用这套剑走偏锋的框架？其实，是受到了周围小伙伴的影响，越来越多的朋友为了解决成本和增加B格，将Blog结合 `GithubPage` 托管在 [`github`](https://github.com/)（程序猿的facebook，我也是从2014年才开始玩的）上，具体这套架构的**优点**和**搭建教程**请参考如下博文：

 - [Blog搭建之Hexo+Node.js+ejs+bootstrap+markdown+GithubPage](http://note.youdao.com/share/?id=0dc251a2004362d10d7ce520fecdcbff&type=note)

   这里主要提一下 `Hexo` 和 `Jacman`：

 - Hexo：
  -  风一般的速度：`Hexo` 基于 `Node.js` ，支持多进程，几百篇文章也可以秒生成。
  -  流畅的撰写：支持 `GitHub` ` Flavored ` ` Markdown ` 和所有 ` Octopress` 的插件。
  -  扩展性：`Hexo`支持 `EJS `、 `Swig `和 `Stylus `。通过插件支持 `Haml`、 `Jade`和 `Less`.
  -  More：[请参见官网](http://hexo.io/)
 

 - Jacman：`Hexo` 的一个 `ejs `模版主题，来源于民间大神__WuChong__（[更多详细用法传送门](http://note.youdao.com/share/?id=d93d060ce27c0d085021c9c0192c9e08&type=note)），在所有Hexo官网主题中个人比较倾向于他的风格，但是对于这个主题，我还是不太满意，挺多瑕疵需要我自己慢慢修改，后面我的Blog主题也会自己改成一个新的原创主题，到时候再开源出来，给更多的 `hexo` 开发者使用。现在正在往 `Low Poly Style`发展。



&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**持续更新中**
