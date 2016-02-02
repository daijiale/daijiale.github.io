title: 【PHP】PHP安全性开发
date: 2015-9-21 21:21:11
tags:

 - PHP


categories:

 - 转载博文
 - Web开发分享

---


# 前言 #

>php给了开发者极大的灵活性，但是这也为安全问题带来了潜在的隐患，近期需要总结一下以往的问题，在这里借翻译一篇文章同时加上自己开发的一些感触总结一下。
<!-- more -->

# 简介

当开发一个互联网服务的时候，必须时刻牢记安全观念，并在开发的代码中体现。PHP脚本语言对安全问题并不关心，特别是对大多数没有经验的开发者来说。每当你讲任何涉及到钱财事务等交易问题时，需要特别注意安全问题的考虑，例如开发一个论坛或者是一个购物车等。

## 安全保护一般性要点

### 不相信表单

对于一般的Javascript前台验证，由于无法得知用户的行为，例如关闭了浏览器的javascript引擎，这样通过POST恶意数据到服务器。需要在服务器端进行验证，对每个php脚本验证传递到的数据，防止XSS攻击和SQL注入

### 不相信用户

要假设你的网站接收的每一条数据都是存在恶意代码的，存在隐藏的威胁，要对每一条数据都进行清理

### 关闭全局变量

在php.ini文件中进行以下配置：

register_globals = Off
如果这个配置选项打开之后，会出现很大的安全隐患。例如有一个process.php的脚本文件，会将接收到的数据插入到数据库，接收用户输入数据的表单可能如下：

```html
<input name="username" type="text" size="15" maxlength="64">
```

这样，当提交数据到`process.php`之后，php会注册一个`$username`变量，将这个变量数据提交到`process.php`，同时对于任何POST或GET请求参数，都会设置这样的变量。如果不是显示进行初始化那么就会出现下面的问题：

```php
<?php
// Define $authorized = true only if user is authenticated
if (authenticated_user()) {
    $authorized = true;
}
?>
```
此处，假设authenticated_user函数就是判断$authorized变量的值，如果开启了register_globals配置，那么任何用户都可以发送一个请求，来设置$authorized变量的值为任意值从而就能绕过这个验证。

所有的这些提交数据都应该通过PHP预定义内置的全局数组来获取，包括$_POST、$_GET、$_FILES、$_SERVER、$_REQUEST等，其中$_REQUEST是一个$_GET/$_POST/$_COOKIE三个数组的联合变量，默认的顺序是$_COOKIE、$_POST、$_GET。

推荐的安全配置选项
error_reporting设置为Off：不要暴露错误信息给用户，开发的时候可以设置为ON

safe_mode设置为Off

register_globals设置为Off

将以下函数禁用：system、exec、passthru、shell_exec、proc_open、popen

open_basedir设置为 /tmp ，这样可以让session信息有存储权限，同时设置单独的网站根目录

expose_php设置为Off

allow_url_fopen设置为Off

allow_url_include设置为Off

### SQL注入攻击

对于操作数据库的SQL语句，需要特别注意安全性，因为用户可能输入特定语句使得原有的SQL语句改变了功能。类似下面的例子：

$sql = "select * from pinfo where product = '$product'";
此时如果用户输入的$product参数为：

39'; DROP pinfo; SELECT 'FOO
那么最终SQL语句就变成了如下的样子：

select product from pinfo where product = '39'; DROP pinfo; SELECT 'FOO'
这样就会变成三条SQL语句，会造成pinfo表被删除，这样会造成严重的后果。

这个问题可以简单的使用PHP的内置函数解决：

$sql = 'Select * from pinfo where product = '"' 
       mysql_real_escape_string($product) . '"';
防止SQL注入攻击需要做好两件事：

对输入的参数总是进行类型验证

对单引号、双引号、反引号等特殊字符总是使用mysql_real_escape_string函数进行转义

但是，这里根据开发经验，不要开启php的Magic Quotes，这个特性在php6中已经废除，总是自己在需要的时候进行转义。

### 防止基本的XSS攻击

XSS攻击不像其他攻击，这种攻击在客户端进行，最基本的XSS工具就是防止一段javascript脚本在用户待提交的表单页面，将用户提交的数据和cookie偷取过来。

XSS工具比SQL注入更加难以防护，各大公司网站都被XSS攻击过，虽然这种攻击与php语言无关，但可以使用php来筛选用户数据达到保护用户数据的目的，这里主要使用的是对用户的数据进行过滤，一般过滤掉HTML标签，特别是a标签。下面是一个普通的过滤方法：

```php
function transform_HTML($string, $length = null) {
// Helps prevent XSS attacks
    // Remove dead space.
    $string = trim($string);
    // Prevent potential Unicode codec problems.
    $string = utf8_decode($string);
    // HTMLize HTML-specific characters.
    $string = htmlentities($string, ENT_NOQUOTES);
    $string = str_replace("#", "#", $string);
    $string = str_replace("%", "%", $string);
    $length = intval($length);
    if ($length > 0) {
        $string = substr($string, 0, $length);
    }
    return $string;
}
```
这个函数将HTML的特殊字符转换为了HTML实体，浏览器在渲染这段文本的时候以纯文本形式显示。如<strong>bold</strong>会被显示为：

&lt;STRONG&gt;BoldText&lt;/STRONG&gt;

上述函数的核心就是htmlentities函数，这个函数将html特殊标签转换为html实体字符，这样可以过滤大部分的XSS攻击。

但是对于有经验的XSS攻击者，有更加巧妙的办法进行攻击：将他们的恶意代码使用十六进制或者utf-8编码，而不是普通的ASCII文本，例如可以使用下面的方式进行：

```html
<a href="http://host/a.php?variable=%22%3e %3c%53%43%52%49%50%54%3e%44%6f%73%6f%6d%65%74%68%69%6e%67%6d%61%6c%69%63%69%6f%75%73%3c%2f%53%43%52%49%50%54%3e">
```

这样浏览器渲染的结果其实是：

```html
<a href="http://host/a.php?variable="> <SCRIPT>Dosomethingmalicious</SCRIPT>
</a>
```
这样就达到了攻击的目的。为了防止这种情况，需要在transform_HTML函数的基础上再将#和%转换为他们对应的实体符号，同时加上了$length参数来限制提交的数据的最大长度。

### 使用SafeHTML防止XSS攻击

上述关于XSS攻击的防护非常简单，但是不包含用户的所有标记，同时有上百种绕过过滤函数提交javascript代码的方法，也没有办法能完全阻止这个情况。

目前，没有一个单一的脚本能保证不被攻击突破，但是总有相对来说防护程度更好的。一共有两个安全防护的方式：白名单和黑名单。其中白名单更加简单和有效。

一种白名单解决方案就是SafeHTML，它足够智能能够识别有效的HTML，然后就可以去除任何危险的标签。这个需要基于HTMLSax包来进行解析。

安装使用SafeHTML的方法：

1、前往http://pixel-apes.com/safehtml/?page=safehtml 下载最新的SafeHTML

2、将文件放入服务器的classes 目录，这个目录包含所有的SafeHTML和HTMLSax库

3、在自己的脚本中包含SafeHTML类文件

4、建立一个SafeHTML对象

5、使用parse方法进行过滤

```php
<?php
/* If you're storing the HTMLSax3.php in the /classes directory, along
   with the safehtml.php script, define XML_HTMLSAX3 as a null string. */
define(XML_HTMLSAX3, '');
// Include the class file.
require_once('classes/safehtml.php');
// Define some sample bad code.
$data = "This data would raise an alert <script>alert('XSS Attack')</script>";
// Create a safehtml object.
$safehtml = new safehtml();
// Parse and sanitize the data.
$safe_data = $safehtml->parse($data);
// Display result.
echo 'The sanitized data is <br />' . $safe_data;
?>
```
SafeHTML并不能完全防止XSS攻击，只是一个相对复杂的脚本来检验的方式。

使用单向HASH加密方式来保护数据

单向hash加密保证对每个用户的密码都是唯一的，而且不能被破译的，只有最终用户知道密码，系统也是不知道原始密码的。这样的一个好处是在系统被攻击后攻击者也无法知道原始密码数据。

加密和Hash是不同的两个过程。与加密不同，Hash是无法被解密的，是单向的；同时两个不同的字符串可能会得到同一个hash值，并不能保证hash值的唯一性。

MD5函数处理过的hash值基本不能被破解，但是总是有可能性的，而且网上也有MD5的hash字典。

### 使用mcrypt加密数据

MD5 hash函数可以在可读的表单中显示数据，但是对于存储用户的信用卡信息的时候，需要进行加密处理后存储，并且需要之后进行解密。

最好的方法是使用mcrypt模块，这个模块包含了超过30中加密方式来保证只有加密者才能解密数据。

```php
<?php
$data = "Stuff you want encrypted";
$key = "Secret passphrase used to encrypt your data";
$cipher = "MCRYPT_SERPENT_256";
$mode = "MCRYPT_MODE_CBC";
function encrypt($data, $key, $cipher, $mode) {
// Encrypt data
return (string)
            base64_encode
                (
                mcrypt_encrypt
                    (
                    $cipher,
                    substr(md5($key),0,mcrypt_get_key_size($cipher, $mode)),
                    $data,
                    $mode,
                    substr(md5($key),0,mcrypt_get_block_size($cipher, $mode))
                    )
                );
}
function decrypt($data, $key, $cipher, $mode) {
// Decrypt data
    return (string)
            mcrypt_decrypt
                (
                $cipher,
                substr(md5($key),0,mcrypt_get_key_size($cipher, $mode)),
                base64_decode($data),
                $mode,
                substr(md5($key),0,mcrypt_get_block_size($cipher, $mode))
                );
}
?>

```
mcrypt函数需要以下信息：

1、待加密数据

2、用来加密和解密数据的key

3、用户选择的加密数据的特定算法（cipher：如 MCRYPT_TWOFISH192,MCRYPT_SERPENT_256， MCRYPT_RC2, MCRYPT_DES, and MCRYPT_LOKI97）

4、用来加密的模式

5、加密的种子，用来起始加密过程的数据，是一个额外的二进制数据用来初始化加密算法

6、加密key和种子的长度，使用mcrypt_get_key_size函数和mcrypt_get_block_size函数可以获取

如果数据和key都被盗取，那么攻击者可以遍历ciphers寻找开行的方式即可，因此我们需要将加密的key进行MD5一次后保证安全性。同时由于mcrypt函数返回的加密数据是一个二进制数据，这样保存到数据库字段中会引起其他错误，使用了base64encode将这些数据转换为了十六进制数方便保存。

[参考文献：http://www.codeproject.com/Articles/363897/PHP-Security](http://www.codeproject.com/Articles/363897/PHP-Security)


# 更多参考
- [PHP中文手册](http://php.net/manual/zh/)


