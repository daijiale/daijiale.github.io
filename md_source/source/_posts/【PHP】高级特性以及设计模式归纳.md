title: 【PHP】高级特性以及设计模式归纳
date: 2016-09-29 15:33:11
tags:

 - PHP


 
categories:

 - 原创博文
 - Web开发笔记
 
---


>PS：之前在和Mentor一起开发新项目时候，接触到了一些很经典的PHP设计模式和高级特性，通过项目沉淀和业余时间的学习，想对常用设计模式进行一个归纳，进行新知识的梳理，方便服务于未来更大型的后端项目，以及帮助更多的PHPer上路。

<!--more-->

# PSR

FIG制定的PHP规范，简称PSR，是PHP开发的事实标准，现在主流的PHP框架：symfony、laravel、yii、thinkphp、百度odp等都严格遵守这个规范。

## PSR-0 自动加载

 - 命名空间必须与绝对路径一致。
 - 类名首字母必须大写。
 - 除入口文件外，其他".php"必须只有一个类。
 
 
###  开发符合PSR-0规范的基础框架

 - 严格全部使用命名空间。
 - 所有PHP文件必须自动载入，不能有include/require。
 - 单一入口。

## PSR-1 基本代码规范

 [PSR-1规范详细介绍](https://segmentfault.com/a/1190000002521577)
 
## PSR-2 代码样式

[PSR-2代码样式详细介绍](https://my.oschina.net/u/1420250/blog/193971)

## PSR-3 日志接口

[PSR-3日志接口详细介绍](https://segmentfault.com/a/1190000002521644)

## PSR-4 自动加载升级版

[PSR-4 PHP规范](https://segmentfault.com/a/1190000000380008)

# SPL

## SPL（PHP标准库）

- SqlStack，SqlQueue，SplHeap，SqlFixedArray等数据结构类。
- ArrayIterator、AppendIterator、Countable、ArrayObject。
- SPL提供的函数。

想深入了解SPL标准库的话，这里有一篇[一峰](http://www.ruanyifeng.com/blog/)前辈的博文：[SPL笔记](http://www.ruanyifeng.com/blog/2008/07/php_spl_notes.html)，国内应该找不到比这更好的中文参考文献了。

# PHP链式操作

**eg:** $db ->where()->limit()->order();

不过多解释了，天天用的。

# PHP魔术方法

## `__get/__set`
 
 常用于类的构造方法，当对象的属性不存在时，将自动调用`__set`初始化属性，`__get`返回对象初始化属性。
 

## `__call/__callStatic`

 常用于类的构造方法，当对象的方法不存在时，将自动回调`__call`初始化方法，`__callStatic`针对静态方法。


## `__toString`

 将对象转换成字符串类型。

## `__invoke`

把对象当成函数去使用，则自动回调`__invoke `方法，返回对象传入的参数。

# 工厂模式

工厂模式的最大优点在于创建对象上面，就是把创建对象的过程封装起来，这样随时可以产生一个新的对象。
减少代码进行复制粘帖，耦合关系重，牵一发动其他部分代码。

通俗的说，以前创建一个对象要使用new，现在把这个过程封装起来了。
假设不使用工厂模式：那么很多地方调用类a，代码就会这样子创建一个实例：new a(),假设某天需要把a类的名称修改，意味着很多调用的代码都要修改。



工厂模式的优点就在创建对象上。
工厂模式的优点就在创建对象上。建立一个工厂（一个函数或一个类方法）来制造新的对象,它的任务就是把对象的创建过程都封装起来，
创建对象不是使用new的形式了。而是定义一个方法，用于创建对象实例。

每个类可能会需要连接数据库。那么就将连接数据库封装在一个类中。以后在其他类中通过类名:

为什么引入抽象的概念？
想一想，在现实生活中，当我们无法确定某个具体的东西的时候，往往把一类东西归于抽象类别。
工厂方法：
比如你的工厂叫做“香烟工厂”，那么可以有“七匹狼工厂”“中华工厂”等，但是，这个工厂只生厂一种商品：香烟；
抽象工厂：无法描述它到底生产什么产品，它生产很多类型的产品(所以抽象工厂就会生成子工厂)。
你的工厂是综合型的，是生产“一系列”产品，而不是“一个”，比如：生产“香烟”，还有“啤酒”等。然后它也可以有派生出来的具体的工厂，但这些工厂都是生产这一系列产品，只是可能因为地域不一样，为了适应当地人口味，味道也不太一样。
工厂模式：理解成只生成一种产品的工厂。比如生产香烟的。
工厂方法：工厂的一种产品生产线 。比如键盘的生成过程。



别人会反驳：吃饱了没事干，一定要修改类名称呢？这个说不定。一般都不会去修改类名称。






其实工厂模式有很多变体，抓住精髓才是关键：只要是可以根据不同的参数生成不同的类实例，那么就符合工厂模式的设计思想。




#单例模式

单例模式的要点有三个：

一是某个类只能有一个实例；

二是它必须自行创建这个实例；

三是它必须自行向整个系统提供这个实例。

下面我们讨论下为什么要使用PHP单例模式？

多数人都是从单例模式的字面上的意思来理解它的用途, 认为这是对系统资源的节省, 可以避免重复实例化, 是一种"计划生育". 而PHP每次执行完页面都是会从内存中清理掉所有的资源. 因而PHP中的单例实际每次运行都是需要重新实例化的, 这样就失去了单例重复实例化的意义了. 单单从这个方面来说, PHP的单例的确有点让各位失望. 但是单例仅仅只有这个功能和应用吗? 答案是否定的,我们一起来看看。

1. php的应用主要在于数据库应用, 所以一个应用中会存在大量的数据库操作, 在使用面向对象的方式开发时(废话), 如果使用单例模式, 则可以避免大量的new 操作消耗的资源。

2. 如果系统中需要有一个类来全局控制某些配置信息, 那么使用单例模式可以很方便的实现. 这个可以参看zend Framework的FrontController部分。

3. 在一次页面请求中, 便于进行调试, 因为所有的代码(例如数据库操作类db)都集中在一个类中, 我们可以在类中设置钩子, 输出日志，从而避免到处var_dump, echo。

```
<?php
class EasyFramework_Easy_Mysql{
    protected static $_instance = null;
    private function __construct(){

    }
    public static function getInstance(){
        if (self::$_instance === null){
            self::$_instance = new self();
        }
        return self::$_instance;
    }

    protected function __clone(){

    }

}
$x = EasyFramework_Easy_Mysql::getInstance();
var_dump($x);
?>

/*
 * 1.第一步：
 * 既然是单例，也就是只能实例化一次，也就代表在实例化时
 * 不可能使用new关键字！！！！
 * 在使用new关键字时，类中的构造函数将自动调用。
 * 但是，如果我们将构造函数的访问控制符设置为protected或private
 * 那么就不可能直接使用new关键字了！！！
 * 第二步：
 * 无论protected/private修饰的属性或方法，请问在当前类的
 * 内部是否可以访问？---> 可以
 * 第三步：
 * 现在我们根本没有办法得到对象（因为你不能使用new关键字了），
 * 第四步：静态成员(包括属性或方法)在访问时，只能通过
 * 类名称::属性()
 * 类名称::方法()
 * 第五步：如果我现在存在一个静态方法--> getInstance()
 * 那么在调用时就应写成
 * $object = EasyFramework_Easy_Mysql::getInstance()
 * 如果，getInstance()方法可以得到唯一的一个对象
 * 也就代表是所谓的单例模式了！！！
 * 第六步，怎么让getInstace()只得到一个对象呢？
 * 既然要得到对象，那么肯定是：
 * $variabl = new ????();
 * 我们又知道静态属性的值是可以所有的对象来继承的！！！
 * 静态成员是属于类的，而非对象的！
 * 所以
 * 第七步：声明一个静态的属性，用其存储实例化的对象
 * protectd static $_instance
 *
 * 并且初始值为null
 * 那么我在调用getInstance()方法时，只需要判断其值是否为空即可\
 *
 * public static function getInstance(){
 *     if(self::_instance === null){
 *      self::_instance = new self();
 *  }
 *  return self::_instance;
 * }
 * 在实例时，一定是这样写：
 * $x = EasyFramework_Easy_Mysql::getInstance();
 * 在第一时调用时,类的$_instance这个静态属性值为null,
 * 那么也就代表,getInstance()方法的判断条件为真了，
 * 也就意味着
 * self::$_instance这个成员有了值了！！！
 * 并且还返回这个值
 * $y = EasyFramework_Easy_Mysql::getInstance();
 * 在第二次或第N次调用时，self::$_instance已经有了值了
 * 也就代表getInstance()方法的条件为假了！！！
 * 也就代表其中的程序代表不可能执行了！！！
 * 也就代表将直接返回以前的值了！！！
 */
 
 ```
 
#  注册器模式

单例模式解决的是如何在整个项目中创建唯一对象实例的问题，工厂模式解决的是如何不通过new建立实例对象的方法。 那么注册树模式想解决什么问题呢？ 在考虑这个问题前，我们还是有必要考虑下前两种模式目前面临的局限。 首先，单例模式创建唯一对象的过程本身还有一种判断，即判断对象是否存在。存在则返回对象，不存在则创建对象并返回。 每次创建实例对象都要存在这么一层判断。 工厂模式更多考虑的是扩展维护的问题。 总的来说，单例模式和工厂模式可以产生更加合理的对象。怎么方便调用这些对象呢？而且在项目内如此建立的对象好像散兵游勇一样，不便统筹管理安排埃因而，注册树模式应运而生。不管你是通过单例模式还是工厂模式还是二者结合生成的对象，都统统给我“插到”注册树上。我用某个对象的时候，直接从注册树上取一下就好。这和我们使用全局变量一样的方便实用。 而且注册树模式还为其他模式提供了一种非常好的想法。

```
class Register{

	protected static $objects;

	public static function set($alias,$object){

		self::$objects[$alias]=$object;

	}

public static function get($alias){

		return self::$objects[$alias];

	}

public static function _unset($alias){

	unset(self::$objects[$alias]);

	}

}

Register::set('rand',RandFactory::factory());

$object=Register::get('rand');

```

# 适配器模式

 - 可以将截然不同的函数接口封装成统一的API。
 - 实际应用举例，PHP的数据库操作有mysql、mysqli、pdo3种、可以用适配器模式统一成一致。类似的场景还有cache适配器，将memcache、redis、file、apc等不同的缓存函数，统一成一致。 
 
 ```
 //目标角色  
interface Target {  
    public function simpleMethod1();  
    public function simpleMethod2();  
}  
  
//源角色  
class Adaptee {  
      
    public function simpleMethod1(){  
        echo 'Adapter simpleMethod1'."<br>";  
    }  
}  
  
//类适配器角色  
class Adapter implements Target {  
    private $adaptee;  
      
      
    function __construct(Adaptee $adaptee) {  
        $this->adaptee = $adaptee;   
    }  
      
    //委派调用Adaptee的sampleMethod1方法  
    public function simpleMethod1(){  
        echo $this->adaptee->simpleMethod1();  
    }  
      
    public function simpleMethod2(){  
        echo 'Adapter simpleMethod2'."<br>";     
    }   
      
}  
  
//客户端  
class Client {  
      
    public static function main() {  
        $adaptee = new Adaptee();  
        $adapter = new Adapter($adaptee);  
        $adapter->simpleMethod1();  
        $adapter->simpleMethod2();   
    }  
}  
  
Client::main();
 
 
 ```
 
 
#  策略模式

- 将一组特定的行为和算法封装成类，以适应某些特定的上下文环境，这种模式就是策略模式。
- 实际应用举例，假如一个电商网站系统，针对男性女性用户要各自跳转到不同的商品类目，并且所有广告位展示不同的广告。
- 实现Ioc，依赖倒置，控制反转。

```
<?php  
/** 
* 策略模式 
* 定义一系列的算法,把每一个算法封装起来, 并且使它们可相互替换。本模式使得算法可独立于使用它的客户而变化 
* 
*/   
  
  
/** 
* 出行旅游 
* 
*  
*/  
interface TravelStrategy{  
    public function travelAlgorithm();  
}   
  
  
/** 
 * 具体策略类(ConcreteStrategy)1：乘坐飞机 
 */  
class AirPlanelStrategy implements TravelStrategy {  
    public function travelAlgorithm(){  
        echo "travel by AirPlain", "<BR>\r\n";   
    }  
}   
  
  
/** 
 * 具体策略类(ConcreteStrategy)2：乘坐火车 
 */  
class TrainStrategy implements TravelStrategy {  
    public function travelAlgorithm(){  
        echo "travel by Train", "<BR>\r\n";   
    }  
}   
  
/** 
 * 具体策略类(ConcreteStrategy)3：骑自行车 
 */  
class BicycleStrategy implements TravelStrategy {  
    public function travelAlgorithm(){  
        echo "travel by Bicycle", "<BR>\r\n";   
    }  
}   
  
  
  
/** 
 *  
 * 环境类(Context):用一个ConcreteStrategy对象来配置。维护一个对Strategy对象的引用。可定义一个接口来让Strategy访问它的数据。 
 * 算法解决类，以提供客户选择使用何种解决方案： 
 */
   
class PersonContext{
  
    private $_strategy = null;  
  
    public function __construct(TravelStrategy $travel){  
        $this->_strategy = $travel;  
    }  
    /** 
    * 旅行 
    */  
    public function setTravelStrategy(TravelStrategy $travel){  
        $this->_strategy = $travel;  
    }  
    /** 
    * 旅行 
    */  
    public function travel(){  
        return $this->_strategy ->travelAlgorithm();  
    }  
}   
  
// 乘坐火车旅行  
$person = new PersonContext(new TrainStrategy());  
$person->travel();  
  
// 改骑自行车  
$person->setTravelStrategy(new BicycleStrategy());  
$person->travel();  
  
?>   
 

```
## 控制反转(IOC)，依赖注入（DI）

eg：[谈谈PHP实现依赖注入(控制反转)](https://my.oschina.net/cxz001/blog/533166)

# 数据对象映射模式（ORM）

- 数据对象映射模式，是将对象和数据存储映射起来，对一个对象的操作会映射为对数据存储的操作。
- 在代码中实现数据对象映射模式，我们将实现一个ORM类，将复杂的SQL语句映射成对象属性的操作。
- 结合使用数据对象映射模式，工厂模式，注册模式。

# 观察者模式
- 观察者模式（Observer），当一个对象状态发生改变时，依赖它的对象全部会收到通知，并自动更新。
- 场景：一个事件发生后，要执行一连串更新操作。传统的编程方法，就是在事件的代码之后直接加入处理逻辑。当更新的逻辑增多之后，代码会变得难以维护。这种方式是耦合的，侵入式的，增加新的逻辑需要修改事件主体的代码。
- 观察者模式实现了低耦合，非侵入式的通知与更新机制。

```
<?php
/**
 * 观察者模式
 * @author: Mac
 * @date: 2012/02/22
 */
 
 
class Paper{ 
	/* 主题    */
    private $_observers = array();
 
    public function register($sub){ /*  注册观察者 */
        $this->_observers[] = $sub;
    }
 
     
    public function trigger(){  /*  外部统一访问    */
        if(!empty($this->_observers)){
            foreach($this->_observers as $observer){
                $observer->update();
            }
        }
    }
}
 
/**
 * 观察者要实现的接口
 */
interface Observerable{
    public function update();
}
 
class Subscriber implements Observerable{
    public function update(){
        echo "Callback\n";
    }
}
 

下面是测试代码

/*  测试    */
$paper = new Paper();
$paper->register(new Subscriber());
//$paper->register(new Subscriber1());
//$paper->register(new Subscriber2());
$paper->trigger();

```


# 原型模式
- 与工厂模式作用类似，都是用来创建对象的。
- 与工厂模式的实现不同，原型模式是先创建好一个原型对象，然后通过clone原型对象来创建新的对象。这样就免去了类创建时重复的初始化操作。
- 原型模式适用于大对象的创建，创建一个大对象需要很大的开销，如果每次new就会消耗很大，原型模式仅需内存拷贝即可。


# 装饰器模式

- 装饰器模式（Decorator），可以动态地添加修改类的功能。
- 一个类提供了一项功能，如果要在修改并增加额外的功能，传统的编程模式，需要写一个子类继承它，并重新实现类的方法。
- 使用装饰器模式，仅仅需在运行时添加一个装饰器对象即可实现，可以实现最大的灵活性。

```
<?php
abstract class Beverage{
    public $_name;
    abstract public function Cost();
}
// 被装饰者类
class Coffee extends Beverage{
    public function __construct(){
        $this->_name = 'Coffee';
    }   
    public function Cost(){
        return 1.00;
    }   
}
// 以下三个类是装饰者相关类
class CondimentDecorator extends Beverage{
    public function __construct(){
        $this->_name = 'Condiment';
    }   
    public function Cost(){
        return 0.1;
    }   
}
 
class Milk extends CondimentDecorator{
    public $_beverage;
    public function __construct($beverage){
        $this->_name = 'Milk';
        if($beverage instanceof Beverage){
            $this->_beverage = $beverage;
        }else
            exit('Failure');
    }   
    public function Cost(){
        return $this->_beverage->Cost() + 0.2;
    }   
}
 
class Sugar extends CondimentDecorator{
    public $_beverage;
    public function __construct($beverage){
        $this->_name = 'Sugar';
        if($beverage instanceof Beverage){
            $this->_beverage = $beverage;
        }else{
            exit('Failure');
        }
    }
    public function Cost(){
        return $this->_beverage->Cost() + 0.2;
    }
}
 
// Test Case
//1.拿杯咖啡
$coffee = new Coffee();
 
//2.加点牛奶
$coffee = new Milk($coffee);
 
//3.加点糖
$coffee = new Sugar($coffee);
 
printf("Coffee Total:%0.2f元\n",$coffee->Cost());

```

# 迭代器模式

- 迭代器模式，在不需要了解内部实现的前提下，遍历一个聚合对象的内部元素。
- 相比于传统的编程模式，迭代器模式可以隐藏遍历元素的所需的操作。

```
<?php  
/** 
 * Created by PhpStorm. 
 * User: Jiang 
 * Date: 2015/6/8 
 * Time: 21:31 
 */  
  
//抽象迭代器  
abstract class IIterator  
{  
    public abstract function First();  
    public abstract function Next();  
    public abstract function IsDone();  
    public abstract function CurrentItem();  
}  
  
//具体迭代器  
class ConcreteIterator extends IIterator  
{  
    private $aggre;  
    private $current = 0;  
    public function __construct(array $_aggre)  
    {  
        $this->aggre = $_aggre;  
    }  
    //返回第一个  
    public function First()  
    {  
        return $this->aggre[0];  
    }  
  
    //返回下一个  
    public function  Next()  
    {  
        $this->current++;  
        if($this->current<count($this->aggre))  
        {  
            return $this->aggre[$this->current];  
        }  
        return false;  
    }  
  
    //返回是否IsDone  
    public function IsDone()  
    {  
        return $this->current>=count($this->aggre)?true:false;  
    }  
  
    //返回当前聚集对象  
    public function CurrentItem()  
    {  
        return $this->aggre[$this->current];  
    }  
} 


//客户端调用

header("Content-Type:text/html;charset=utf-8");  
//--------------------------迭代器模式-------------------  
require_once "./Iterator/Iterator.php";  
$iterator= new ConcreteIterator(array('周杰伦','王菲','周润发'));  
$item = $iterator->First();  
echo $item."<br/>";  
while(!$iterator->IsDone())  
{  
    echo "{$iterator->CurrentItem()}：请买票！<br/>";  
    $iterator->Next();  
}  

```

# 代理模式

- 在客户端与实体之间建立一个代理对象（proxy），客户端对实体进行操作全部委派给代理对象，隐藏实体的具体实现细节。

- Proxy还可以与业务代码分离，部署到另外的服务器，业务代码中通过RPC来委派任务。

```
<?php
class Printer {   //代理对象,一台打印机
    public function printSth() {
      	 echo 'I can print <br>';
    }
                
    // some more function below
    // ...
}
             
class TextShop {    //这是一个文印处理店,只文印,卖纸,不照相
	private $printer;
                
    public function __construct(Printer $printer) {
         $this->printer = $printer;
    }
                
    public function sellPaper() {    //卖纸
          echo 'give you some paper <br>';
    }
                
    public function __call($method, $args) {    //将代理对象有的功能交给代理对象处理
         if(method_exists($this->printer, $method)) {
            $this->printer->$method($args);
         }
    }
}
             
class PhotoShop {    //这是一个照相店,只文印,拍照,不卖纸
      private $printer;
                
      public function __construct(Printer $printer) {
           $this->printer = $printer;
      }
                
      public function takePhotos() {    //照相
           echo 'take photos for you <br>';
      }
                
      public function __call($method, $args) {    //将代理对象有的功能交给代理对象处理
           if(method_exists($this->printer, $method)) {
               $this->printer->$method($args);
           }
       }
}
            
     $printer = new Printer();
     $textShop = new TextShop($printer);
     $photoShop = new PhotoShop($printer);
            
     $textShop->printSth();
     $photoShop->printSth();
?>

```



# 面向对象编程的基本原则

 - 1.单一职责：一个类，只需要做好一件事情。
 - 2.开放封闭：一个类，应该是可扩展的，而不可修改的。
 - 3.依赖倒置：一个类，不应该强依赖另一个类，每个类对于另一个类都是可替换的。
 - 4.配置化：尽可能地使用配置，而不是硬编码。
 - 5.面向接口编程：只需要关心接口，不需要关心实现。
 






























