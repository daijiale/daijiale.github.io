title: AjaxReview
date: 2015-4-9 18:30:09

tags:

- Ajax
- Json
- Javascript


categories:

- 原创博文
- Web开发日记


---



**PS：**写这篇Blog主要有两个三个动机：

 - 对以前自己所学知识的一个系统化review
 - 刚好给徒弟漫漫制作一份比较全面的Ajax教程
 - 帮助更多的，对此知识有需要的童鞋
<!--more-->

<embed src="http://music.163.com/style/swf/widget.swf?sid=29561077&type=2&auto=1&width=320&height=66" width="340" height="86"  allowNetworking="all"></embed>
# 概念介绍 #
   在介绍`Ajax`的概念之前，我觉得有必要先说明一下**“同步”和“异步”**的问题：

## 同步： ##

举个例子——“在以前，我们填写100个字段的网页表单(需要联系人，密码，姓名，用户名等等)最后，我们点击提交，结果发现等了5分钟，服务器告诉你邮箱字段填写错误，然后很自然的提交失败，打回重新写过..."（来回三次以上我估计你差不多就疯了）

这就是一个**同步流程**：

> **p1-pn:客户端浏览器请求->等待服务器端处理->之后响应->页面才能载入更新状态**

无疑这种机制对于现实来说很扯蛋。所以有了下面的**异步机制**。

## 异步： ##
还是上面的例子：在填写表单的过程中，每填写一个字段就同时像服务器提交一次请求，在你编辑其他内容的同时，也会同步你的请求，随时得到服务器端反馈的结果，可以实时保存你填写的字段内容，也可以及时得到填写错误的反馈。

**异步流程：**

> **p1:客户端->请求->服务端处理->响应->页面载入**
> 
> **p2:客户端->请求->服务端处理->响应->页面载入**
> 
> **p3:客户端->请求->服务端处理->响应->页面载入**
> 
> **......**
> 
> **pn:客户端->请求->服务端处理->响应->页面载入**

这种异步方式起源于`XMLHttpRequest`对象的诞生，具体信息请戳链接[百度百科——XMLHTTPRequest](http://baike.baidu.com/link?url=URNRyvoOrqtF_qSUI2iiYgpkhs2_N5aFFwjdRnwJ9ReioxiBvdkERVP3Uvb6QXYziaSHv28GLklZ0VXolav0J_)，这里就不重点介绍了...

## Ajax的概念： ##
即**Asyn-Javascript-XML**，通过在后台与服务器进行少量数据交换，这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。（即实现异步机制）


# Ajax学习前需要具备的基础 #

 - 运用HTML和CSS来实现页面，表达信息
 - 运用XMLHttpRequest和Web服务器进行数据的异步交互
 - 运用JS操作DOM，实现动态局部刷新
 
**PS**：（这些都不会就可以不用往下看了）


# XHR的使用  #

首先声明XHR对象

```
 var request = new XMLHttpRequest//不支持IE5 IE6

```

`珍爱生命，远离IE`（虽然嘴上是这样说）

```
var request;
if（windowXMLHttpRequest）{
 	request = new XMLHttpRequest();//IE7+;Firefox;Chrome,Opera,Safari...
	} else {
		request=new ActiveXObject("Microsoft.XMLHTTP");//IE5,IE6
	}
}
```

## 这里再简单介绍下HTTP： ##
其实就是一群 `叫兽` 研究出来的一套计算机通信规则。

它的特点需要我们注意：

 - 不建立持久连接，
 - 无状态，
 - 不稳定，
 - 无记忆
 
因此，我们一般都只用 `http` 来充当 `请求` 操作，通过 `TCP` 连接来建立持久化的网络通信。

**一个完整的HTTP请求过程7步骤**：

 - 1.建立 `TCP` 连接
 - 2.Web浏览器向服务器发送 `请求` 命令
 - 3.Web浏览器发送请求头信息
 - 4.Web浏览器应答
 - 5.Web服务器发送应答头信息
 - 6.Web服务器向浏览器发送数据
 - 7.Web服务器关闭 `TCP` 连接


**HTTP请求结构**：

 - 1.HTTP请求的动作：`GET` or `POST` 
    - `GET`
      - 一般用于信息获取
      - 使用URL传递参数
      - 对所发信息的数量有限制（2000个字符左右）
    - `POST`
      - 一般用于修改服务器上的资源。
      - 对所发信息的数量无限制
 - 2.正在请求的 `URL` ，总得知道请求地址吧
 - 3.请求头，包含一些客户端环境信息，身份验证信息等
 - 4.请求体，也就是请求正文，可以包含客户提交的查询字符串信息，表单信息等等

**HTTP响应组成**：

 - 1.一个**数字和文字**组成的**状态码**，用来显示成功还是失败
    - 状态码：
     - 1xx：信息类，表示收到web浏览器请求，正在进一步处理中
     - 2xx：成功，200 ok
     - 3xx：重定向，表示请求没有成功，客户必须采取进一步动作
     - 4xx：客户端错误，表示客户端提交的请求有错误，如 404 Not Found
     - 5xx：服务器错误，不能完成对请求的处理，如 500
 - 2.响应头，包含服务器类型，日期时间，内容类型和长度等
 - 3.响应体，也就是响应正文

## XHR发送请求 ##
```
request.open(method,url,async);
request.send(string);
```
**eg:**
```
request.open("POST","127.0.0.1/post.php",true);
request.setRequestHeader("Content-type","application/x-www-form-urlencoded");
request.send("name=daijiale&sex=man");
```
## XHR取得响应 ##
```
responseText//获得字符串形式的响应数据
responseXML//获得XML形式的响应数据
status//以数字形式返回HTTP状态码
statusText//以文本形式返回HTTP状态码
getALLResponseHeader()//获取所有响应报头
getResponseHeader()//查询响应中的某个字段的值
```

这里重点需要关注下 `readyState` 属性
 
 - 0：请求未初始化，open还没有调用
 - 1：服务器连接已建立，open已经调用
 - 2：请求已接受，也就是收到了头信息
 - 3：请求处理中，也就是接收到响应主体了
 - 4：请求已完成，且响应已就绪，也就是响应完成了

因此，通过监听此属性的值，可以得到对应响应结果。方法如下：
```
var request = new XMLHttpRequest();
request.open("GET","127.0.0.1/get.php",true);
request.send();
request.onreadystatechange = function(){
if(request.readyState===4 && request.status===200）｛
//do some thing here 
//request.responseText
	}
｝
```
# Ajax Demo #
 - [Ajax加载XML](http://www.w3school.com.cn/tiy/t.asp?f=ajax_httprequest_js)
 - [Ajax实现Web通信 ](http://www.w3school.com.cn/ajax/ajax_example_suggest.asp)
 - [Ajax加载html表格元素](http://www.w3school.com.cn/tiy/t.asp?f=ajax_httprequest_js_4)
 - [PHP-MySQL使用Ajax进行数据交互](http://www.w3school.com.cn/php/php_ajax_database.asp)





#JQuery Ajax
很多初学者，很容易把这 `Jquery Ajax` 和 `Ajax`搞混，这里单独给大家区分一下。
首先，我们先了解下: 
## JQuery 和 ajax 的关系： ##
 
 - `jQuery` 提供多个与 `AJAX` 有关的方法。
 - 通过 `jQuery AJAX` 方法，您能够使用 `HTTP Get` 和 `HTTP Post` 从远程服务器上请求 `文本`、`HTML`、`XML` 或 `JSON`
 - 同时能够把这些外部数据直接载入网页的被选元素中。
 - 如果没有 `jQuery`，`AJAX` 编程还是有些难度的。
 - 编写常规的 `AJAX` 代码并不容易，因为不同的浏览器对 `AJAX` 的实现并不相同。这意味着您必须编写额外的代码对浏览器进行测试。不过，`jQuery 团队`为我们解决了这个难题，我们只需要一行简单的代码，就可以实现 `AJAX` 功能。
 - **综上： `jQuery` 对 `Ajax` 进行了又一次的封装，使得其主体功能用起来更为方便，简单，现在大部分开发者都很少直接用js写原生ajax，都是引入了jquery ajax方法。但是由于依赖于`jQuery`，导致不灵活，有其局限性。**


##JQuery Ajax的使用方法##
基本语法格式：

```

$(document).ready(function(){
  $("#button_name").click(function(){
   $.ajax({
		type:"POST",
		url:"127.0.0.1/get.php",
        data:{
		    name:$("#staffname").val(),
			password:$("#staffpassword").val(),
		},
        dataType:"json",
		success:function(data){
			if(data.success){
				$("#button_click_result").html(data.msg);
				}else{
				$("#button_click_result").html("error"+data.msg);
				}
			},
		error:function(jqXHR){
			alert("error："+jqXHR.status);
			}
		})
	})
}

```

jQuery load()//简单而强大

```
$(selector).load(URL,data,callback);
```


load()方法直接从服务器加载数据，并把返回的数据放入被选元素中

```
<!DOCTYPE html>
<html>
<head>
<script src="/jquery/jquery-1.11.1.min.js"></script>
<script>
$(document).ready(function(){
  $("button").click(function(){
$("#div1").load("/example/jquery/demo_test.txt",function(responseTxt,statusTxt,xhr){
  if(statusTxt=="success")
alert("外部内容加载成功！");
  if(statusTxt=="error")
alert("Error: "+xhr.status+": "+xhr.statusText);
});
  });
});
</script>
</head>
<body>

<div id="div1"><h2>使用 jQuery AJAX 来改变文本</h2></div>
<button>获得外部内容</button>

</body>
</html>
```
本例[Demo效果](http://www.w3school.com.cn/tiy/t.asp?f=jquery_ajax_load_callback)

更多请参见 [jQuery Ajax操作函数表](http://www.w3school.com.cn/jquery/jquery_ref_ajax.asp)

#JSON
 这里补充一下这个知识，和xml一样，经常在ajax数据传输中用到，没什么难的，看看就会了：

 - 概念：javascript对象表示法
 - JSON是存储和交换文本信息的语法，类似XML。采用键值对的方式来组织，易于人们阅读和编写，同时也易于机器解析和生成。
 - JSON是独立于语言的，也就是说不管什么语言，都可以解析json，只需要按照json的规则来就行。


##  与XML进行对比：  ##
 
 - json的长度和xml格式比起来很短小
 - json读写的速度更快
 - json可以使用JavaScript内建的方法直接进行解析，转换成Javascript对象，非常方便。


## 书写类型： ##
**eg：**"name":"戴嘉乐"


## JSON解析： ##
不要使用 `eval` 来解析，`eval`会解析内置函数，造成安全隐患.尽量用 `JSON.parse`.


## JSON校验工具： ##

 - [JSONLint](http://jsonlint.com/)


 `json` 更多教材：[传送门](http://www.w3school.com.cn/json/)


#  跨域问题  #
之前在长虹实习的时候，和海阳遇到了这样的问题，这里也拎出来提一下。
## 什么是跨域？ ##
 - 一个域名地址的组成：
	- `http:// ` 协议
	- `www.` 子域名
	- `abc.com` 主域名
	- `:8080` 端口号
	- `script/jquery.js` 请求资源地址
 - 当协议、子域名、主域名、端口号中任意一个不相同时，都算作不同域。
 - 不同域之间相互请求资源，就算做**“跨域”**
    - eg：http://www.daijiale.cn/index.html请求http://daijiale.github.io/service.php

**PS:**

Javascript处出去安全方面的考虑，不允许跨域调用其他页面的对象（这不废话，肯定不允许啊）

所以很多新手在本机调试程序的时候容易乱写 `127.0.0.1` 和 `localhost:8080` ，这也算作跨域

协议不同（`http` 和  `https` 也算作跨域)


## 处理跨越访问的方法 ##

### 一、代理 ###

 - 通过在同域名的web服务器端创建一个代理：
 - 北京服务器www.beijing.com 上海服务器www.shanghai.com
 - 通过北京web服务器后台（www.beijing.com/proxy-shanghaiservice.php)来调用上海服务器（www.shanghai.com/service.php)的服务，然后再把响应结果返回给前端，这样前端调用北京同域名的服务就和调用上海的服务效果相同了。

### 二、JSONP ###
`JSONP` 可用于解决**主流**浏览器的跨域数据访问的问题。

**eg：**

在www.aaa.com页面中：

```
<script>
function jsonp(json){

	alert(json["name"]):
}
</script>
<script src="http://www.bbb.com/jsonp.js"></script>

```

在www.bbb.com页面中：

```
jsonp({'name':'洪七','age':24});

```

JSONP只支持 `GET` 请求，存在局限性：

```

$(document).ready(function(){
  $("#button_name").click(function(){
   $.ajax({
		type:"GET",//JSONP只支持GET,存在局限性
		url:"127.0.0.1/get.php",
        dataType:"jsonp",//主要修改这
		jsonp:"callback",//前端脚本语言需要新建变量对象来获取名为“callback”的jsonp
		success:function(data){
			if(data.success){
				$("#button_click_result").html(data.msg);
				}else{
				$("#button_click_result").html("error"+data.msg);
				}
			},
		error:function(jqXHR){
			alert("error："+jqXHR.status);
			}
		})
	})
}

```


### 三、XHR2 ###
 
 - html5提供的XMLHttpRequest Level2 已经实现了跨域访问以及其他的一些新功能
 - IE10以下版本不支持
 - 在服务器端做一些小小的改造即可：
    - header('Access-Control-Allow-Origin:*');
    - header('Access-Control-Allow-Methods:POST,GET');


还是那句话 `珍爱生命，远离IE`，强烈推荐第三种方法解决跨域，低成本。
# 反向Ajax技术 #

关于 `Reverse Ajax` 我会单独另起一篇博文对该技术进行介绍，之前在自己的“Web远程控制系统”中用到了该技术，觉得很有必要提及一下：
[传送门正在更新中]()


# 总结 #

Ajax在web前端开发中的地位不言而喻，高频的使用率以及其不可替代的定位是每个前端工程师都无法质疑的。现在，类似于Jquery Ajax的快速开发框架也层出不穷，对于我们来说无疑是福音，但是理解其内涵和工作原理，我们才能在以后的实践项目中应用得当。下面是一些Ajax的常用场景：

### 场景1. 用Ajax进行数据验证###
在填写表单内容时，需要保证数据的唯一性（例如新用户注册填写的用户名），因此必须对用户输入的内容进行数据验证。

数据验证通常有两种方式：

一种是直接填写，然后提交表单，这种方式需要将这个页面提交到服务器端进行验证，整个过程不仅时间长而且造成了服务器不必要的负担；

第二种方式是改进了的验证过程，用户可以通过点击相应的验证按钮，打开新窗口查看验证结果，但是这样需要新开一个浏览器窗口或者对话框，还需要专门编写验证的页面，比较耗费系统资源。

而使用Ajax技术，可以由XMLHttpRequest对象发出验证请求，根据返回的HTTP响应判断验证是否成功，整个过程不需要弹出新窗口，也不需要将整个页面提交到服务器，快速而又不加重服务器负担。

### 场景2.按需取数据 ###

分类树或者树形结构在web应用系统中使用得非常广泛，例如部门结构，文档得分类结构常常使用树形空间呈现。

以前每次对分类树得操作都会引起页面重载，为了避免这种情况出现，一般不采用每次调用后台得方式，而是一次性将分类结果中得数据一次性读取出来并写入数组，然后根据用户的操作，用JavaScript来控制节点的呈现，这样虽然解决了操作响应速度，不重复载入页面以及避免向服务器频繁发送请求的问题，但是如果用户不对分类进行操作或者只对分类树中的一部分数据进行操作的话（这种情况很普遍的），那么读取的数据中就会有相当大的冗余，浪费了用户的资源。特别是在分类结构复杂，数据庞大的情况下，这种弊端就更加明显了。

现在应用Ajax改进分类树的实现机制。在初始化页面时，只获取第一级子分类的数据并且显示；当用户点开一级分类的第一节点时，页面会通过Ajax向服务器请求当前分类所属的二级子分类的所有数据；如果再请求已经呈现的二级分类的某一节点时，再次向服务器请求当前分类所属的三级子分类的所有数据，以此类推。页面会根据用户的操作向服务器请求它所需要的数据，这样就不会存在数据的冗余，减少了数据下载总量。同时，更新页面时不需要重载所有内容，只更新需要更新的那部分内容即可，相对于以前后台处理并且重载的方式，大大缩短了用户的等待时间。

### 场景3.自动更新页面 ###

在web应用中有很多数据的变化时十分迅速的，例如最新的热点新闻，天气预报以及聊天室内容等。在Ajax出现之前，用户为了即使了解相应的内容必须不断刷新页面，查看是否有新的内容变化，或者页面本身实现定时刷新的功能（大多数聊天室页面就是这样做的）。

有可能会发生这种情况；有一段时间网页的内容没有发生任何变化，但是用户并不知道，仍然不断的刷新页面；或者用户失去了耐心，放弃了刷新页面，却很有可能在此有新的消息出现，这样就错过了第一时间得到消息的机会。

应用Ajax可以改善这种这种情况，页面加载以后，会通过Ajax引擎在后台进行定时的轮询，向服务器发送请求，查看是否有最新的消息。如果有则将新的数据（而不是所有数据）下载并且在页面上进行动态的更新，通过一定的方式通知用户（实现这样的功能正是JavaScript的强项）。

这样即避免了用户不断手工刷新页面的不便，也不会因为重复刷新页面造成资源浪费。

既然你己经对Ajax产生了兴趣，还要知道重要的一点，即什么时候应该使用Ajax技术，而什么时候不该用。首先，不要害怕在应用中尝试新的方法。我们相信.几乎每个web应用都能从Ajax技术中获益，只不过不要矫杠过正，过于离谱就行了。从验证开始就很合适，但是不要限制你的主动性。你当然可以使用Ajax提交数据，但也许不能把它作为提交数据的主要方法。

其次，惟一会影响你应用Ajax的就是浏览器问题。如果大量用户(或者特别重要的用户)还在使用比较旧的浏览器，如IE 5 , Safari 1.2或Mozilla 1.0之前的版本，Ajax技术就不能奏效。如果这是一些很重要的用户，你就要使用针对目标用户的跨浏览器的方法，而放弃Ajax，或者开发一个可以妥善降级的网站。浏览器支持可能不是一个重要因素，因为netscape Navigaxar 4在市场上的份额很小。不过，还是应该查看Web日志，看看你的应用适用什么技术。

如前所述，验证和表单填写就非常适合采用Ajax实现。还可以使用DOM的“拖”技术建立真正动态的网站，如google的个性化主页.

可以看到，Ajax为Web应用开发提供了新的机会。你不会再因为以往的专用技术或技术折中方案而受到妨碍。利用Ajax，胖客户与瘦客户之间的界限不再分明，真正的赢家则是你的用户。




> 更多Ajax与PHP与json跨域交互的Demo源码，请参考我的github项目:[DaiJiale's Ajax Demo](https://github.com/daijiale/DaiJiale-Front-End/tree/master/ajax_demo)

>原创博文
>
>作者：戴嘉乐 
>
>转载请注明出处，谢谢！


