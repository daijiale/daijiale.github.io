title: 【Android】常用Adb Shell命令
date: 2015-5-6 16:23:09
tags:

 - Android
 
categories:

 - 原创博文
 - Android开发笔记
 
---

# 实用Adb Shell命令演示 #
> 一些灵巧方便的Adb Shell命令笔记


## Adb命令的主要用途 ##
 - 运行Android设备的命令行。
 - 管理模拟器或Android设备的映射端口。
 - 安装和卸载应用程序。
 - 计算机和Android设备之间的上传和下载文件。
 
<!--more-->
## Adb操作命令 ##

1. 显示系统中全部Android平台：
2. 
```
    android list targets
```

2. 显示系统中全部AVD（模拟器）：

	```
    android list avd
	```

3. 创建AVD（模拟器）：
```
    android create avd --name 名称 --target 平台编号
```

4. 启动模拟器：
```
    emulator -avd 名称 -sdcard ~/名称.img (-skin 1280x800)
```

5. 删除AVD（模拟器）：
```
    android delete avd --name 名称
```

6. 创建SDCard：
```
    mksdcard 1024M ~/名称.img
```

7. AVD(模拟器)所在位置：
```
    Linux(~/.android/avd)      
    Windows(C:\Documents and Settings\Administrator\.android\avd)
```

8. 启动DDMS：
```
ddms
```

9. 显示当前运行的全部模拟器：
```
    adb devices
```

10. 对某一模拟器执行命令：
```
      abd -s 模拟器编号 命令
```

11. 安装应用程序：
```
      adb install -r 应用程序.apk
```
12. 获取模拟器中的文件：
```
      adb pull <remote> <local>
```

13. 向模拟器中写文件：
```
      adb push <local> <remote>
```

14. 进入模拟器的shell模式：
```
      adb shell
```

15. 启动SDK，文档，实例下载管理器：
```
      android
```

16. 缷载apk包：
```
      adb shell
      cd data/app
      rm apk包
      exit
      adb uninstall apk包的主包名
      adb install -r apk包
```

17. 查看adb命令帮助信息：
```
      adb help
```

18. 在命令行中查看LOG信息：
```
      adb logcat -s 标签名
```

19. adb shell后面跟的命令主要来自：
      源码\system\core\toolbox目录和源码\frameworks\base\cmds目录。

20. 删除系统应用：
      ```
	  adb remount （重新挂载系统分区，使系统分区重新可写）。
      adb shell
      cd system/app 
      rm *.apk
```

21. 获取管理 员权限：
```
      adb root
```

22. 启动Activity：
```
      adb shell am start -n 包名/包名＋类名（-n 类名,-a action,-d date,-m MIME-TYPE,-c category,-e 扩展数据,等）。
```

23、发布端口：
    你可以设置任意的端口号，做为主机向模拟器或设备的请求端口。如： 

```
adb forward tcp:5555 tcp:8000
```

24、复制文件：
    你可向一个设备或从一个设备中复制文件， 
     复制一个文件或目录到设备或模拟器上： 
 
```
 adb push <source> <destination></destination></source> 
```      
如：
```
adb push test.txt /tmp/test.txt 
```     
从设备或模拟器上复制一个文件或目录： 
```     
adb pull <source> <destination></destination></source> ```
如：
```
adb pull /addroid/lib/libwebcore.so .
```

25、搜索模拟器/设备的实例：

```
     取得当前运行的模拟器/设备的实例的列表及每个实例的状态：
    adb devices
```

26、查看bug报告： 

```
adb bugreport 
```

27、记录无线通讯日志：

```
    一般来说，无线通讯的日志非常多，在运行时没必要去记录，但我们还是可以通过命令，设置记录： 
    adb shell 
    logcat -b radio
```

28、获取设备的ID和序列号：

```
     adb get-product 
     adb get-serialno
```

29、访问数据库SQLite3

```
adb shell 
sqlite3
```

## Adb 高级命令 ##

 - am – ActivityManager 命令
 - pm – PackageManager命令
 - wm – WindowManager 命令
 - content – ContentProvider命令



### am命令 ###
 - am 子命令
 - 可以启动Activity，Service
 - 可以停止一个应用
 - 可以发送broadcast
 - 可以监控activity的状态
 

```
 am start –n <ComponentName>
 am start –a <Action> -c <Category> -e<Extra>
```

例：

```

am start –n com.android.settings //启动系统设置

am start –a com.android.intent.action.MAIN –c com.android.intent.category.HOME //启动Home页

am force-stop <package>

am start -a android.intent.action.VIEW -d http://www.baidu.com

am start -a android.intent.action.CALL -d tel:12345

am startservice –n <ComponentName>

am broadcast …

am force-stop <package>


```

### pm命令 ###

 - pm install  – 安装一个apk (可以用adb install)
 - pm uninstall [-k] – 删除一个apk (可以用adb uninstall)
 - pm list  -- 列出一系列包，Feature或者permission等
 - pm path <PACKAGE>  -- 获得某个包的apk文件路径
 - pm clear <PACKAGE>  -- 清除某个应用的数据
 - pm enable <Component> --启用某个组件
 - pm disable <Component> -- 禁用某个组件




 
### wm命令 ###

 - wm (WindowManager)命令，主要查看屏幕的信息
 - wm size  -- 获取屏幕尺寸
 - wm density – 获取屏幕密度
 - wm overscan – 过扫描
 


### content命令 ###
 
 - 操作ContentProvider的命令
 - content insert – 向content provider中插入数据
 - content query – 请求content provider中的数据
 - content update – 更新content provider中的数据
 - content delete – 删除 content provider中的数据
 - *数据值之间用冒号(:)分隔


### settings命令 ###

```
settings put global my_key “hello”
settings get global my_key 

```
### media媒体控制命令 ###

```
media dispatch <PLAY, PAUSE, STOP …>
```

模拟一个媒体按键的发送

