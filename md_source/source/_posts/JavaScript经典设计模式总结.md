title: JavaScript经典设计模式总结
date: 2015-6-9 11:29:31
tags:

 - Javascript

categories:

 - 原创博文
 - Web开发日记

---


# 前言 #

> 在百度即将离职的最后一个月里，我被派往新项目担任FE的角色，借此机会也重新review一下大一大二的老本行——Web前端开发。这篇Blog是对自己所学2年JS的一个设计模式总结，也是实际编码中经常用到的很多经典Case，希望能帮助到更多JS开发者，也给那些还活跃在 `JS API` 层的同学打开一个新的学习思路。
PS：本篇Blog侧重点在于JS的设计模式，如果只是期望了解JS语法，请直接戳[W3C传送门](http://www.w3school.com.cn/js/index.asp)！
<!--more-->


# 一、JS基础 #

## 动态类型语言和鸭子类型

## this、call、apply
Javascript 的`this`总是指向一个对象，而具体指向哪个对象是在运行时基于函数的执行环境动态绑定的，而不是函数被声明时的环境。

### this的指向
除去不常用的`with`和`eval`情况。

#### 1、作为对象的方法调用
当函数作为对象的方法被调用时候，this指向该对象：

```
var obj = {
a: 1,
getA: function(){
	alert( this === obj); // 输出:true
	alert( this .a); //输出:1
	｝
};
obj.getA();

```

#### 2、作为普通函数调用
即普通函数方式：this总是指向全局对象，在浏览器的JavaScript中，这个全局对象是window对象。

```
window.name = 'globalName' ;

var myObject = {
		name: 'daijiale' , 
		getName = function(){
		return this.name;
		}
};
var getName = myObject.getName;
console.log( getName() ); //输出: globalName


```
注：
有时候我们在某个标签，例如“div”节点的事件函数内部，有一个局部的callback方法，callback被作为普通函数调用时，callback内部的this指向了window，如下：

```
<html>
		<body>
			<div id = "div1">我是一个div</div>
		</body>
		
		<script>
		
		window.id = 'window' ;
		
		document.getElementById( 'div1' ).onclick = function(){
			alert(this.id); //输出：'div1'
			var callback = function(){
				alert(this .id); //输出:'window'
			}
			callback();
		};
		</script>
</html>
```
但我们往往是想让他指向div节点，解决办法：可以用一个变量保存div节点的引用：

```
document.getElementById('div1').onclick = function(){
		var that = this; //保存div的引用
		var callback = function(){
				alert(that.id);	//输出： ‘div1’
		
	}
	callback();
};
```


#### 3、构造器调用

JavaScript中没有类，但是可以从构造器中创建对象，同时也提供了ne运算符，使得构造器看起来更像一个类。
当用new运算符调用函数时，该函数总会返回一个对象，通常情况下，构造器里的this就指向返回的这个对象，见如下代码:

```
var MyClass ＝ function(){
		this.name = ' daijiale';
				
}；

var obj = new MyClass();
alert(obj.name); //输出daijiale;

```
注:
如果构造器显式地返回了一个object类型的对象，那么此次运算结果最终会返回这个对象，而不是我们之前期待的this:

```
var MyClass = function(){
	this.name = 'daijiale';
       return {  //显示地返回一个对象
         name:'daijiale2'
       }	
};

var obj = new MyClass();
alert(obj.name); //输出daijiale2
```

#### 4、Function.prototype.call 或 Function.prototype.apply调用
跟普通的函数调用相比，用Function.prototype.call或 Function.prototype.apply可以动态的改变传入函数的this:

```
var obj1 = {
	name : 'daijiale',
	getName: function(){
		return this.name;
	}
};

var obj2 = {
	name: 'daijiale2'
};

console.log(obj1.getName());//输出：daijiale
console.log(obj1.getName.call(obj2));//输出:daijiale2

```



#最后汇总一些比较好的技术文章和其他资料
 - [javascript异步编程的4种方法](http://www.w3cfuns.com/blog-5465288-5408787.html)