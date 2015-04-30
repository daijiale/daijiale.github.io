title: AndroidWear开发笔记——进军可穿戴领域
date: 2015-5-1 16:23:09
tags:

 - Android
 - AndroidWear
 - 智能可穿戴
 
categories:

 - 原创博文
 - Android开发笔记
---

#本文作者#

![](http://7xi6qz.com1.z0.glb.clouddn.com/djlblogpicslrme.jpg)

### [Jiale Dai](http://www.daijiale.cn) ###

电子科技大学 本科生

从14年开始一直专研于AndroidWear开发

百度 MBU RD

百度[DuWear](http://duwear.baidu.com)团队成员



#写在开头#
##对AndroidWear自己的看法：##

Android Wear的目标就是：不接触手机的前提下，在你需要的时候，它把对你有用的信息呈现给你，扫一眼就够了。ta是
一种新的交互模型，有很多有利便捷新潮的交互体验是手机上无法实现的。将你自己置身于一个外部场景，在移动和忙碌中使用这项服务是什么样的体验，你就会发现ta的价值。
### 核心元素： ###

 - Google Now：用户可以和AndroidWear“说话”（语音交互）。
 - Notifications:一个卡片，一个方块，实现你最想要的服务。具体分为stacks、 pages、 replies、三种性质。
 - WatchFace：表之所以称之为“表”。
 - Data Message：和手机的数据通信机制是重要的桥梁。

### 构建一个Wear Apps的基础（wear app能做到什么？）： ###

 - Custom UI
 - Send Data
 - Control Sensors
 - Voice Actions


下面我会举**四个例子**来说明基于这几个基础元素（Android Wear API）可以实现的强大的，精致的轻量级wear app：
 

 - Camera
    - Wearable DataApi 
	- Wearable MessageAPI
	- WatchActivity 
- Maps
  - Voice Actions 
  - Custom Display Intent Notifications

#### Gmail ####
 - Gmail Base On
    - Notification Bundles
    - RemoteInput for Voice Response
 
大家应该不会陌生Gmail，下面来看看其在wear端的app特性：

1、首先ta有两种邮件提醒类型：

**单页通知卡片（图）**，

**多页通知卡片（图）**，

2、你可以通过滑动卡片，进一步了解更多信息，而且伴随有 **“语音快速回复” “归档” “在手机端打开回复”**三种操作，这里我们重点谈一下**语音快速回复**这个**具有wear特性**的操作流程：

 - 
`Android Notification API` 会让你通过远程输入给reply行为做一个注释,而远程输入则会告诉AndroidWear，在执行这个行为之前，你要把文本输入的方式改为语音，因此当Gmail建立一个notification连接时，wear端会给reply行为附加一个远程输入，AndroidWear会看到这条远程输入，然后不会立即发送一个行为，会首先启动一个wear UI界面来收集语音回复信息，然后把转换好的文本变成意图，再发送意图到你的手机上，手机得到意图后，就可以在不触动手机UI的情况下发送/回复邮件了。

下面是关键代码实现过程：

**Add RemoteInput to Reply Action**
```
Action replyAction = new NotificationCompat.Action.Builder(
	R.drawble.ic_reply,getString(R.string.reply), 
	replyPendingIntent)
    addRemoteInput(
	  new RemoteInput.Builder(EXTRA_REPLY_TEXT)
		.setLabel(getString(R.string.replyLabel))
		.build()).build();
```

**Modify Activity to Use Reply Text**

```
Bundle results =
	RemoteInput.getResultsFromIntent(intent);
	if(results!=null){
	 String message = 
		results getString(EXTRA_REPLY_TEXT)
	}
```


3、最后我再来详细介绍下“多卡片重叠式信息提醒”的实现原理，视觉设计上采用的是复线收件箱的风格，ta不再把多条信息压缩到单一的卡片中，我们想做的是每封邮件都有自己的卡片。而这些卡片又放入一个可扩大的堆栈中，这一堆提醒卡片，也叫提醒卡片堆栈，也是notification API的新特性，不会把所有邮件提醒都只能通过一个提醒显示出来，而是按类划分，表明有所关联，ta们在可穿戴设备上组合成一个卡片丛，而用户也可以通过卡片丛去逐个浏览，以提取某一封邮件，并对其回复或者进行其他操作。而卡片丛 即：notification group也有一个分类键，通过设置这个键来控制丛内卡片顺序，并且可以从中标记一个卡片作为组群的整体摘要描述，具体实现代码如下：


```
Notification card1 = 
	new NotificationCompat.Builder(context)
		.setGroup(GROUP_KEY)
		.setSortKey("0")
		.build();

Notification card2 = 
	new NotificationCompat.Builder(context)
		.setGroup(GROUP_KEY)
		.setSortKey("1")
		.build();

Notification card3 = 
	new NotificationCompat.Builder(context)
		.setGroup(GROUP_KEY)
		.setSortKey("2")
		.build();

Notification summary = 
	new NotificationCompat.Builder(context)
		.setGroup(GROUP_KEY)
		.setGroupSummar;
		.build();
```

#### Hangouts ####
 - Hangouts
    - Custom Wearable Actions
    - Notification Pages