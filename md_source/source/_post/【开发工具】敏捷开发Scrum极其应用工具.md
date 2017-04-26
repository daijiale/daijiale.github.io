title: 【开发工具】敏捷开发Scrum极其应用工具
date: 2016-06-08 21:34:01
tags:

 - 敏捷开发

 
categories:

 - 原创博文
 - Web开发笔记
 
---


>PS：记录之前BIT学院培训后的积累，梳理了敏捷开发之Scrum的知识，并且推荐一些对于初创团队低成本高可用的团队协作工具。

<!--more-->
# Scrum？

Scrum的英文意思是橄榄球运动的一个专业术语，表示“争球”的动作；把一个开发流程的名字取名为Scrum，我想你一定能想象出你的开发团队在开发一个项目时，大家像打橄榄球一样迅速、富有战斗激情、人人你争我抢地完成它，你一定会感到非常兴奋的。

它是一种开发方法，也就是一种软件开发的流程，它会指导我们用规定的环节去一步一步完成项目的开发；而这种开发方式的主要驱动核心是人；它采用的是迭代式开发。

# Sprint？

Sprint是短距离赛跑的意思，这里面指的是一次迭代，而一次迭代的周期是1个月时间（即4个星期），也就是我们要把一次迭代的开发内容以最快的速度完成它，这个过程我们称它为Sprint。

# 3Roles

## 产品负责人（Product Owner）

主要负责确定产品的功能和达到要求的标准，指定软件的发布日期和交付的内容，同时有权力接受或拒绝开发团队的工作成果。

## 流程管理员（Scrum Master）

主要负责整个Scrum流程在项目中的顺利实施和进行，以及清除挡在客户和开发工作之间的沟通障碍，使得客户可以直接驱动开发。

## 开发团队（Scrum Team）

主要负责软件产品在Scrum规定流程下进行开发工作，人数控制在5~10人左右，每个成员可能负责不同的技术方面，但要求每成员必须要有很强的自我管理能力，同时具有一定的表达能力；成员可以采用任何工作方式，只要能达到Sprint的目标。

# 4Meetings

## Sprint Planning

### 目标
 - 关键里程碑
 - 确定产品发布周期
 - 确定Sprint周期
 
### 步骤
 - PO讲解需求 
 - 估算Story相对工作量
 - 评估优先级
 - 确定迭代Backlog
 
## Daily Scrum

### 每日例会：对Sprint进行检查和调整。

 - 每天15分钟的状态汇报会议。
 - 每天在同一个时间同一个地点。
 - 每人三个问题：
 	- 上次会议之后做了什么。
 	- 下次会议之前要做什么。
 	- 有什么困难？
 - 团队更新Sprint Backlog和Sprint燃尽图。
 - 对所有人开放，但是只有PO可以说话。


## Showcase

### Sprint 评审会议：对产品的检查和调整。

 - 团队演示完成的工作和未完成的工作。
 - 从产品负责人和干系人那里得到反馈。
 - 更新产品Backlog和发布燃尽图。
 

## Retro

### Sprint 回顾会议：对流程的检查和调整。

 - 团队对过去一个Sprint中的人、关系、流程和工具做检查。
 - 团队确定可能的改进并对这些改进在下一个Sprint的度量标准达成共识。

# 3Tools

## Product Backlog

产品功能需求列表：

 - 演进的，排序的，预估的。
 - 越高优先级越详细。
 - 产品负责人来维护，但是任何人都可以贡献想法。
 - 每个产品一个列表。
 
## Sprint Backlog

能够把product backlog变成可用产品功能的任务。

  - 由团队创建并在sprint中维护。
  - 每个人都可以添加、删除、改变Sprint Backlog。
  - 团队成员自发认领任务，而没有人指派。
  - 任务用小时估计，通常是1-16小时。
  - 每天估计剩余工作量。

## Burndown Charts

Sprint燃尽图：显示Sprint中的剩余工作量。

 - 以小时计算。
 - 每日更新



# Scrum Start

![](http://7xi6qz.com1.z0.glb.clouddn.com/ScrumModel.jpg)

- 1、我们首先需要确定一个Product Backlog（按优先顺序排列的一个产品需求列表），这个是由Product Owner 负责的；

- 2、Scrum Team根据Product Backlog列表，做工作量的预估和安排；

- 3、有了Product Backlog列表，我们需要通过 Sprint Planning Meeting（Sprint计划会议） 来从中挑选出一个Story作为本次迭代完成的目标，这个目标的时间周期是1~4个星期，然后把这个Story进行细化，形成一个Sprint Backlog；

- 4、Sprint Backlog是由Scrum Team去完成的，每个成员根据Sprint Backlog再细化成更小的任务（细到每个任务的工作量在2天内能完成）；

- 5、在Scrum Team完成计划会议上选出的Sprint Backlog过程中，需要进行 Daily Scrum Meeting（每日站立会议），每次会议控制在15分钟左右，每个人都必须发言，并且要向所有成员当面汇报你昨天完成了什么，并且向所有成员承诺你今天要完成什么，同时遇到不能解决的问题也可以提出，每个人回答完成后，要走到黑板前更新自己的 Sprint burn down（Sprint燃尽图）；

- 6、做到每日集成，也就是每天都要有一个可以成功编译、并且可以演示的版本；很多人可能还没有用过自动化的每日集成，其实TFS就有这个功能，它可以支持每次有成员进行签入操作的时候，在服务器上自动获取最新版本，然后在服务器中编译，如果通过则马上再执行单元测试代码，如果也全部通过，则将该版本发布，这时一次正式的签入操作才保存到TFS中，中间有任何失败，都会用邮件通知项目管理人员；

- 7、当一个Story完成，也就是Sprint Backlog被完成，也就表示一次Sprint完成，这时，我们要进行 Srpint Review Meeting（演示会议），也称为评审会议，产品负责人和客户都要参加（最好本公司老板也参加），每一个Scrum Team的成员都要向他们演示自己完成的软件产品（这个会议非常重要，一定不能取消）；

- 8、最后就是 Sprint Retrospective Meeting（回顾会议），也称为总结会议，以轮流发言方式进行，每个人都要发言，总结并讨论改进的地方，放入下一轮Sprint的产品需求中；

# Scrum场景图

## 任务卡片

![](http://7xi6qz.com1.z0.glb.clouddn.com/%E4%BB%BB%E5%8A%A1%E5%8D%A1%E7%89%87.png)

![](http://7xi6qz.com1.z0.glb.clouddn.com/%E4%BB%BB%E5%8A%A1%E5%8D%A1%E7%89%872.png)
eg:Product Backlog示例。

## 每日站会
![](http://7xi6qz.com1.z0.glb.clouddn.com/%E6%AF%8F%E6%97%A5%E7%AB%99%E4%BC%9A.png)

![](http://7xi6qz.com1.z0.glb.clouddn.com/%E7%AB%99%E4%BC%9A:%E8%AF%84%E5%AE%A1%E4%BC%9A.png)
eg:参会人员可以随意姿势站立，任务看板要保证让每个人看到，当每个人发言完后，要走到任务版前更新自己的燃尽图.

## 任务看板
![](http://7xi6qz.com1.z0.glb.clouddn.com/%E4%BB%BB%E5%8A%A1%E7%9C%8B%E6%9D%BF.png)

![](http://7xi6qz.com1.z0.glb.clouddn.com/%E4%BB%BB%E5%8A%A1%E7%9C%8B%E6%9D%BF2.png)
eg:任务看版包含 未完成、正在做、已完成 的工作状态，假设你今天把一个未完成的工作已经完成，那么你要把小卡片从未完成区域贴到已完成区域。


## 计划纸牌
![](http://7xi6qz.com1.z0.glb.clouddn.com/%E8%AE%A1%E5%88%92%E7%BA%B8%E7%89%8C.png)
eg：它的作用是项目在开发过程中，让成员被能人所领导。怎么用的呢？比如A程序员开发一个功能，需要5个小时，B程序员认为只需要半小时，那他们各自取相应的牌，藏在手中，最后摊牌，如果时间差距很大，那么A和B就可以讨论A为什么要5个小时...

## 燃尽图
![](http://7xi6qz.com1.z0.glb.clouddn.com/%E7%87%83%E5%B0%BD%E5%9B%BE.png)
eg: 任务看版包含 未完成、正在做、已完成 的工作状态，假设你今天把一个未完成的工作已经完成，那么你要把小卡片从未完成区域贴到已完成区域。

# Intro Some Economic and Effective Cases And Tools

## In Baidu

- icafe：Scrum推动工具
- Agile：持续集成工具
- iCode：版本控制工具
- Cooder：代码评审工具
- BaiduWiki：Wiki/周报工具
- BaiduHi：IM工具

## For More Entrepreneurial Teams Like [Fundin](http://www.fundin.cn)

如果追求分布式开展工作模块：

- [Worktile](https://worktile.com/blog/pro/agile-management?hmsr=toutiao.io&utm_medium=toutiao.io&utm_source=toutiao.io)/[EasyPM](https://easypm.cn/team/2281/filter/-280)：Scrum推动工具
- [TravisCI](https://travis-ci.org/)：持续集成工具
- [Coding]()/[Github](https://github.com/)：版本控制工具
- [Reviewable](https://reviewable.io/)：代码评审工具
- [Tower](https://tower.im/)：Wiki/周报工具
- [Slack](https://slack.com/)：IM工具

如果追求集中式开展工作模块：[码云](http://git.oschina.net/)

# 参考文献
- [瀑布/迭代/敏捷模型对比](http://blog.csdn.net/orclight/article/details/8642585)
- [Scrum扫盲篇](http://www.cnblogs.com/taven/archive/2010/10/17/1853386.html)
 
 
 
 
 
