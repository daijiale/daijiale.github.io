title: AndroidWear开发笔记1——进军可穿戴领域
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
> **对AndroidWear自己的看法：**
> 
> Android Wear的目标就是：不接触手机的前提下，在你需要的时候，它把对你有用的信息呈现给你，扫一眼就够了。ta是
> 一种新的交互模型，有很多有利便捷新潮的交互体验是手机上无法实现的。将你自己置身于一个外部场景，在移动和忙碌中使用这项服务是什么样的体验，你就会发现ta的价值。

<!-- more -->


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


下面我会举**四个例子**来说明基于这几个基础元素（Android Wear API）可以实现的强大的，精致的轻量级**wear app**：
 

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
 - Hangouts Base On
    - Custom Wearable Actions
    - Notification Pages
    
1、环聊信息也会自动桥接到可穿戴设备上，在Gmail上面，我们想要的是语音回复，但是环聊的提醒则稍微有点不同，ta并没有回复行为，只是一个内容意图，只需要打开App就可以键入回复了，所以环聊可以很好的不依赖手机而直接在可穿戴设备上进行体验，而且，android wear的 `Notification API`会让你在手机和可穿戴设备上细化不同的操作设置，即手机行为只在手机显示。wear行为只在wear端显示，这就使得我们可以添加一个仅限可穿戴设备使用的回复行为，这一行为涵盖了一个远程输入，却无需变动手机行为。

2、环聊也增加了新的提醒特征：近期会话历史记录。因为在语音回复之前，多出现一些聊天记录总是好的，为了实现这个效果，我们在wear设备扩充器中采用了添加页面的办法：它可以让你为主要提醒内容增加额外的页面，我们把聊天记录放入一个次级大的文本式提醒，然后把它加入到主要提醒中的第二页，并且手机端的提醒体验同时保持不变。关键代码实现如下：

```
Notification chatHistory =
	new NotificationCompat.Builder()
		.setStyle(
			new NotificationCompat.BigTextStyle()
				.bigText(getChatHistory()))
		.build();

firstPageNotification.extend(
	new NotificationCompat.WearableExtender()
		.addPage(chatHistory)
		.build());

```


####  Google Camera ####

 - Google Camera Base On
    - Wearable DataApi 
	- Wearable MessageAPI
	- WatchActivity 

1、wear端的google camera为相机App增加了好玩有趣的特性：**通过手腕来按下快门**，和很多具有远程遥控的高级相机原理类似，你把手机架在三脚架上或者靠在墙上又或者让其他人帮你拿着，然后你通过按住腕表的一个按键来捕捉一个画面**（代替现在正在热卖而很多大男生却不好意思在大街上用的自拍杆，嘿嘿）**。

2、相比于前面提到的Gmail和环聊（ta们只是用了 `Notification API` 来对手机端的消息做一个整合），Google Camera是一个相对个性而又具有和手机交互的特点，而且对于wear来说，仅仅只需要将**快门**这个按键特殊处理就行，所以，在wear端，光快门按键就霸占整个屏幕这一点也是可以容忍的。

3、通过 `Google Play Service`(以后简称 `GMS`)，实现相机的wear端和手机端通信，在相机手机app准备好拍摄时，手机端端会设置好数据项（意味着它已经做好了接受远程快门信息的准备），这种数据项由手表中wear app内置的服务读取，而wear端则会显示出快门按钮，按住按钮，把信息发回手机来激活手机端的快门键，最后，如何预览你刚才拍到的照片呢？很简单：手机端会创建一张缩略图，然后作为数据项中的一个asset发送回手表端，然后做为wear端全屏来预览。
关键代码如下：

**Setting a DataItem**
```
PutDataMapRequest dataMapRequest =
	PutDataMapRequest.create(DATA_ITEM_NAME);
dataMapRequest.getDataMap().putBoolean(
	FIELD_READY,cameraReady);
Wearable.DataApi.putDataItem(
	mGoogleApiClient,
	dataMapRequest.asPutDataRequest()
);
```
**WearableListenerService**
```
public class CameraListennerService
	extends WearableListennerService{
	@Override
	public void onDataChanged(DataEventBuffer dataEvents){
		for(DataEvent dataEvent:dataEvents){
		    if(dataEvent.getType()== 	DataEvent.TYPE_CHANGED){
			DataMapItem mapDataItem = 
				DataMapItem.fromDataItem(
					dataEvent.getDataItem());
			if(mapDataItem.getDataItem().getBoolean(FIELD_CAMERA_READY,false)){
			postNotification();
		}else{
		stopActivity();	
	}
	)
  }
 }
}
```
**Sending an Asset**

```
PutDataMapRequest dataMapRequest =
	PutDataMapRequest.create(DATA_ITEM_NAME);
dataMapRequest.getDataMap().putBoolean(
	FIELD_READY,cameraReady);

//在DataItem数据项中插入一个判断
if(previewBitmap != null){
	dataMapRequest.getDataMap().putAsset(
	 FIELD_PREVIEW,preview);
	)
}

Wearable.DataApi.putDataItem(
	mGoogleApiClient,
	dataMapRequest.asPutDataRequest()
);
```
#### Google Maps ####
- Google Maps Base On
  - Voice Actions 
  - Custom Display Intent Notifications

1、在Google Maps导航期间，我们想要在手腕上提示导航，这个应用场景在走路的时候尤其有用，因为你要一直拿着你的手机走在大马路上会非常奇怪且占用你的双手，如果把手机放在你的口袋，转而看一下手表的描述来获知导航信息无疑更为便捷和实用。

2、在wear端，google想实现对布局和导航呈现精细的把握，特地搭建了一款wear版本的google maps wear App，让个性抽取式卡片成为限于本地的提醒，通过修改google map手机app的数据项，增加了用于下一次操作的描述与图标以及用于解释导航状态的信息，同时wear端的google maps也增加了这样的数据项，每次变动发生后，ta都会读取新数据，然后更新wear的卡片，提取卡片时，wear app采用可穿戴扩充器的新显示意图特性，你可以指定一个活动来在提醒卡片中绘制内容，这样我们想在卡片上画什么都是可以的，而不是受限制于标准提醒样式。
关键实现代码：

**Custom Notification with Display Intent**
```
Intent displayIntent = 
	createUpdateIntent(data, maneuverBitmap);
displayIntent.setClass(
	this,NotificationDisplayActivity.class);

PendingIntent displayPendingIntent = PendingIntent.getActivity(
	this,0,displayIntent,
	PendingIntent.FLAG_CANCEL_CURRENT);

Notification notification = builder.extend(
	new NotificationCompat.WearableExtender()
		.setHintHideIcon(true)
		.setDisplayIntent(displayPendingIntent)
		.setBackground(background)
		.addPage(secondPage)
		.build();

```

3、google map通过语音指令来开启导航进程，为了实现这一点，可
wear端的google map app会联手意图过滤器（ `Intent`）来为导航声音指令服务，然后需要在 `AndroidManifest.xml`中声明如下：
```
<activity
	android:name=".StarNavigationActivity"
	android:theme="@style/TranslucentTheme">
	<intent-filter>
	 <action
		android:name="android.intent.action.VIEW"/>
		<category			android:name="android.intent.category.DEFAULT"/>
	    <data
			android:scheme="google.navigation"/>
	</intent-filter>
</activity>
```
声明之后，就会产生一个像这样的意图，可穿戴App接收到这个意图之后，就会给手机上的google map发送一条信息，信息包括目的地和导航模式，手机google map app接收到这条信息之后，然后开始导航到目的地，然后就可以出发了，具体通信代码如下:

**Sending a Message**

```
private void startNavigation(Intent intent){
	
	String uriString = intent.getDataString();
	
	mGoogleApiClient.blockingConnect(
		Constants.TIMEOUT_MS,
		TimeUnit.MILLISECONDS);

	DataMap dataMap = new DataMap();
	dataMap.putString(FIELD_URI,uriString）；

	Wearable.MessageApi.sendMessage(
	   mGoogleApiClient,
	   mOtherNodeId,
	   Constants.MESSAGE_PATH_START_NAVIGATION,
		dataMap.toByteArray()).await();
	googleApiClient.diconnect();
}

```

**Receiving a Message**

```
public void onMessageReceived(
	MessageEvent messageEvent){
	if(messageEvent.getPath().equals(
		MESSAGE_PATH_START_NAVIGATION)){
	  DataMap requestData =
			DataMap.fromByteArray(
				messageEvent.getData());
			String uriString =
				requestData.getString(FIELD_URI);
			Intent navIntent = new Intent(
				Intent.ACTION_VIEW,
				Uri.parse(uriString));
			startActivity(navIntent);
	}
}

```


# 搭建云驱动的Android Wear Apps #


![](http://7xi6qz.com1.z0.glb.clouddn.com/djlblog_androidwear_cloud.JPG)


如图：

 - 1、首先，我们需要一款云服务来作为App的后端来进行数据推送和数据处理。
 - 2、其次，移动App会配合这项服务发出一个提醒，而你会在wear设备上看到，而且该提醒也会发送到任何相连的AndroidWear设备上。
 - 3、接着，当然就是AndroidWear App本身了，ta的特效是搭建在移动手机App之内，这样一旦手机App发出一条可以在手机上看见的提醒，这条信息也发给了Android Wear设备，现在的API也能够传送这些提醒，比如说触发回复行为，那么，我们是怎么实现ta们的呢？`Android Studio`实际上把你所需要的一切都给你了，包括用于搭建后端服务的工具包，当然还有Android手机App，现在你还可获得扩展包来操作Android Wear，云后端可以用一款外露的API来搭建，Android Studio工具包可以让你在Java下来进行此类操作，为你处理精细的细节把握，你可以写一段云端代码，通过使用属性，ta能够暴露出运行在你android app中的API，这些属性告诉Android客户端这些代码都是到底在干什么的，比如在执行一种叫quotesApi(引用API）时，并提供一种称为getQuote（获得引用）的方式，ok，一旦你现在搭建好了云服务，借助工具包，事实上你就可以自动创建客户端数据库来进入了，接下来，你要做的当然是搭建你的App了，那如何从你的App进入API呢？其实，这个分类已经自动帮你加载下来了，并且放入Maven库中了，这样你就可以直接在你搭建好的Gradle文件夹下涵盖它们了。
  - 4、最后，现在我们如何把它拓展来用于Android Wear呢？ 其实很简单，跟google map中的例子一样，通过修改notification和卡片的代码，使用wear端的api，让消息提醒和前端信息视图同时展现在手机客户端和wear端，此时的**手机App**就**变成了wear连接云端**的**中间件**。


# 写给设计师们：如何把握Android Wear下App的设计原则？ #

> PS：自己在DuWear项目组除了做RD研发之外，也对设计比较感兴趣，记得研发表盘那段时期，经常和搭档（MUX-UE-大侯）一起交流wear端的设计理念和心得，这里分享一些给想进军wear端的设计师朋友们：


单独写了一篇wear端设计原则的blog，请戳 [传送门]()。


# 如何为Android Wear搭建更高级别的UI #

持续更新中

# 具体来聊聊Android Wear的提醒新特性 #


持续更新中


# Android Wear下的全屏App设计理念 #


持续更新中

# Android Wear 数据层API DevBytes #


持续更新中

# Android Wear 下的表盘设计 #

持续更新中

# 如何开发一款个性的Android Wear表盘 #


持续更新中
