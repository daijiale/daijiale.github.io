title: 【数据库-持续更新】Redis入门
date: 2016-08-01 11:15:21
tags:

 - 数据库
 - Redis

 
categories:

 - 原创博文
 - Web开发笔记
 
---


>PS：最近公司业务需要使用redis做部分数据的缓存，特此学习一下。

<!--more-->


# 概述

Redis是一个开源，先进的key-value存储，并用于构建高性能，可扩展的Web应用程序的完美解决方案。

Redis从它的许多竞争继承来的三个主要特点：

- Redis数据库完全在内存中，使用磁盘仅用于持久性。
- 性能极高 – Redis能读的速度是110000次/s,写的速度是81000次/s 。
- 相比许多键值数据存储，Redis拥有一套较为丰富的数据类型，因为值（value）可以是 字符串(String), 哈希(Map), 列表(list), 集合(sets) 和 有序集合(sorted sets)等类型。
- Redis可以将数据复制到任意数量的从服务器，即master-slave模式的数据备份。


# Redis安装


Ubuntu下：

```shell
sudo apt-get update
sudo apt-get install redis-server
```

启动Redis:

```shell
redis-server
```

通过新进程测试redis是否启动：

```shell
redis-cli
```

127.0.0.1 是本机 IP ，6379 是 redis 服务端口。现在我们输入 PING 命令：

```shell
redis 127.0.0.1:6379>ping
PONG
```

# Redis数据类型

Redis支持五种数据类型：string（字符串），hash（哈希），list（列表），set（集合）及zset(sorted set：有序集合)。

[使用方法](http://www.redis.net.cn/tutorial/3505.html)

# Redis命令

[使用方法](http://www.redis.net.cn/tutorial/3506.html)

# Redis事务
[使用方法](http://www.redis.net.cn/tutorial/3515.html)

# Redis 数据备份与恢复

[使用方法](http://www.redis.net.cn/tutorial/3519.html)

# Redis PHP扩展
[Demo](http://www.redis.net.cn/tutorial/3526.html)








# 参考文档

- [redis脚本实例讲解](http://www.cnblogs.com/w58480513/p/4226176.html?utm_source=tuicool&utm_medium=referral)
- [redis_php库](https://github.com/ukko/phpredis-phpdoc)
- [redis中文网](http://www.redis.net.cn/tutorial/3501.html)